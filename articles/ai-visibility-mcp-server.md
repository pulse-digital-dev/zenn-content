---
title: "Cloudflare WorkersでMCPサーバーを作って、AIエージェントに自社データを食わせる"
emoji: "🔌"
type: "tech"
topics: ["MCP", "CloudflareWorkers", "AI", "API", "TypeScript"]
published: true
---

:::message
🎉 **2026年6月、MCP公式レジストリに登録されました** → [io.github.pulse-digital-dev/mcp-ai-visibility-index](https://registry.modelcontextprotocol.io/servers/io.github.pulse-digital-dev/mcp-ai-visibility-index)
:::

## この記事でやること

AIエージェント（Claude Desktop / Cursor / Cline等）から直接呼び出せる **MCPサーバー** を **Cloudflare Workers** 上に構築する。

実例として、日本EC企業104社のAI検索可視性データを返す[AI Visibility Index](https://ai-visibility-index.dev/)のMCPサーバー実装を全公開する。

**ソースコード**: [pulse-digital-dev/mcp-ai-visibility-index](https://github.com/pulse-digital-dev/mcp-ai-visibility-index)（Public）

```
Claude Desktop → JSON-RPC 2.0 → Cloudflare Worker → KVからデータ取得 → JSON-RPCレスポンス
```

## MCPとは何か（30秒で）

**Model Context Protocol (MCP)** はAnthropicが策定したオープン仕様で、AIモデルが外部ツール・データソースにアクセスするためのプロトコル。

- **仕様**: JSON-RPC 2.0ベース
- **トランスポート**: Streamable HTTP / stdio / SSE
- **バージョン**: `2025-03-26`（最新）

要するに「AIエージェントが叩けるAPI」の標準規格。OpenAPI/Swaggerの次世代版と考えるとわかりやすい。

## アーキテクチャ

```
┌─────────────────┐    POST /mcp     ┌─────────────────────┐
│  Claude Desktop  │ ───────────────→ │  Cloudflare Worker  │
│  Cursor / Cline  │ ←─────────────── │  (Streamable HTTP)  │
└─────────────────┘   JSON-RPC 2.0   └─────────┬───────────┘
                                                │
                                      ┌─────────▼───────────┐
                                      │   Cloudflare KV     │
                                      │ (ランキングデータ)    │
                                      └─────────────────────┘
```

Cloudflare Workersを選んだ理由：

- **コールドスタートなし**（V8 Isolates）— MCPはレスポンス速度が重要
- **無料枠10万リクエスト/日** — 十分すぎる
- **KV統合** — データストアが同一エッジで完結
- **グローバルエッジ** — どこからでも低レイテンシ

## 実装

### 1. wrangler.jsonc（プロジェクト設定）

```jsonc
{
  "name": "ai-visibility-index",
  "main": "src/index.js",
  "compatibility_date": "2025-04-01",
  "routes": [
    { "pattern": "ai-visibility-index.dev/*", "zone_name": "ai-visibility-index.dev" }
  ],
  "kv_namespaces": [
    { "binding": "MCP_KV", "id": "your-kv-id" }
  ]
}
```

### 2. MCP JSON-RPCディスパッチャー（server.js）

MCPサーバーの核心部分。3つのメソッドをハンドルする：

```javascript
// server.js — MCP Server (Streamable HTTP Transport)
import { TOOLS, executeTool } from './tools.js';
import { authenticate, checkRateLimit, checkBurstLimit } from './auth.js';

const SERVER_INFO = {
  name: 'ai-visibility-index',
  version: '1.0.0',
};

export async function handleMCP(request, env) {
  // CORS preflight
  if (request.method === 'OPTIONS') {
    return new Response(null, { status: 204, headers: corsHeaders() });
  }

  // GET — サーバー情報
  if (request.method === 'GET') {
    return jsonResponse({
      name: SERVER_INFO.name,
      version: SERVER_INFO.version,
      protocol: 'MCP 2025-03-26 (Streamable HTTP)',
      tools: TOOLS.length,
    });
  }

  // POST — JSON-RPCリクエスト処理
  const body = await request.json();
  const result = await processJsonRpc(body, request, env);
  return jsonResponse(result);
}

async function processJsonRpc(body, request, env) {
  const { id, method, params } = body;

  // Notifications（id なし）— 受け入れのみ
  if (id === undefined || id === null) return null;

  switch (method) {
    case 'initialize':
      return {
        jsonrpc: '2.0', id,
        result: {
          protocolVersion: '2025-03-26',
          capabilities: { tools: {} },
          serverInfo: SERVER_INFO,
        }
      };

    case 'tools/list':
      return { jsonrpc: '2.0', id, result: { tools: TOOLS } };

    case 'tools/call':
      return handleToolsCall(id, params, request, env);

    default:
      return { jsonrpc: '2.0', id, error: { code: -32601, message: `Method not found: ${method}` } };
  }
}
```

**ポイント**: `initialize` → `tools/list` → `tools/call` の3ステップがMCPの基本フロー。

### 3. ツール定義（tools.js）

MCPの「ツール」はOpenAIのFunction Callingと似た構造。`inputSchema`でJSON Schemaを定義する：

```javascript
// tools.js
export const TOOLS = [
  {
    name: 'check_ai_visibility',
    description: 'Check the AI visibility (LLMO/GEO) score for a specific domain...',
    inputSchema: {
      type: 'object',
      properties: {
        domain: {
          type: 'string',
          description: 'Domain to check (e.g., "zozo.jp")',
        },
      },
      required: ['domain'],
    },
    // MCP 2025-03-26: Tool Annotations
    annotations: {
      title: 'AI Visibility Score Checker',
      readOnlyHint: true,
      idempotentHint: true,
      openWorldHint: false,
    },
  },
  // ... 他のツール
];
```

### 4. 認証 & レートリミット（auth.js）

MCPサーバーは誰でも叩ける。AIエージェントのループ攻撃を防ぐために **秒間レートリミット** は必須：

```javascript
// auth.js — 1リクエスト/秒のバースト制限
export async function checkBurstLimit(keyId, env) {
  if (!env.MCP_KV) return { allowed: true, retryAfterMs: 0 };

  const burstKey = `burst:${keyId}`;
  const lastCall = await env.MCP_KV.get(burstKey);

  if (lastCall) {
    const elapsed = Date.now() - parseInt(lastCall, 10);
    if (elapsed < 1000) {
      return { allowed: false, retryAfterMs: 1000 - elapsed };
    }
  }

  // タイムスタンプを記録（TTL 60秒、CF KV最低値）
  await env.MCP_KV.put(burstKey, String(Date.now()), {
    expirationTtl: 60,
  });

  return { allowed: true, retryAfterMs: 0 };
}
```

**なぜ秒間制限が必要か**: MCPクライアント（特にAIエージェント）は、ユーザーの質問に答えるためにツールを連続呼び出しする。制限なしだと1秒に数十リクエスト飛んでくることがある。

### 5. MCP Discovery（.well-known/mcp.json）

MCP仕様では `/.well-known/mcp.json` でサーバーの存在を広告できる。Cloudflare Workersのルーティングで対応：

```javascript
// .well-known/mcp.json を返す
if (url.pathname === '/.well-known/mcp.json') {
  return jsonResponse({
    mcpServers: {
      'ai-visibility-index': {
        url: 'https://ai-visibility-index.dev/mcp',
        name: 'AI Visibility Index',
        description: 'LLMO/GEO rankings for 104 Japanese EC companies',
      }
    }
  });
}
```

これにより、MCP対応クライアントがドメインからサーバーを自動発見できる。

### 6. Claude Desktopの設定

`claude_desktop_config.json` に追加するだけ：

```json
{
  "mcpServers": {
    "ai-visibility-index": {
      "url": "https://ai-visibility-index.dev/mcp"
    }
  }
}
```

`mcp-remote` パッケージは不要。MCP `2025-03-26` 以降、Streamable HTTPをサポートするクライアントは `url` 直接指定で接続できる。

## 動作確認

curlで3ステップを確認：

```bash
# 1. initialize
curl -s -X POST https://ai-visibility-index.dev/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"test","version":"1.0.0"}}}'

# 2. tools/list
curl -s -X POST https://ai-visibility-index.dev/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}'

# 3. tools/call — ZOZOTOWNのAI可視性を確認
curl -s -X POST https://ai-visibility-index.dev/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"check_ai_visibility","arguments":{"domain":"zozo.jp"}}}'
```

実行結果：

```json
{
  "company": "ZOZOTOWN",
  "domain": "zozo.jp",
  "overallScore": 6.15,
  "ranking": { "overall": "4 / 104", "industry": "1 / 14" },
  "engineScores": {
    "chatgpt": 10.52,
    "claude": 5.39,
    "gemini": 2.04,
    "perplexity": 4.08
  }
}
```

## 実装のハマりどころ

### 1. Notifications を無視するな

MCPクライアントは `initialize` の後に `notifications/initialized` を送ってくる。`id` がないので応答は不要だが、**受け入れないと接続が切れる**：

```javascript
if (id === undefined || id === null) {
  if (method === 'notifications/initialized') return null;
  if (method === 'notifications/cancelled') return null;
  return null;  // 未知の通知も無視
}
```

### 2. KV の最終整合性に注意

Cloudflare KVは結果整合性。バースト制限にKVを使うと、並行リクエストがすり抜ける場合がある。逐次的なエージェントループ（1リクエスト→待つ→次）は確実にブロックできるが、同時並行は通過する可能性あり。

厳密にやるなら Durable Objects が必要だが、MVPではKVで十分。

### 3. Tool Annotations は書いておけ

MCP `2025-03-26` で追加された `annotations` は、エージェントがツールの安全性を判断する材料になる：

```javascript
annotations: {
  readOnlyHint: true,     // データを変更しない
  idempotentHint: true,   // 何度叩いても同じ結果
  openWorldHint: false,   // 閉じたデータセット
}
```

これを書くだけで、AIエージェントが安心してツールを使ってくれる。

## コスト

| 項目 | 料金 |
|------|------|
| Cloudflare Workers | 無料（10万req/日） |
| Cloudflare KV | 無料（10万read/日） |
| カスタムドメイン | 年$10程度 |
| **合計** | **ほぼ¥0** |

## まとめ

- MCPサーバーは **JSON-RPC 2.0のエンドポイントを1つ作るだけ** で実装できる
- Cloudflare Workers なら **コールドスタートなし・無料枠で十分** 運用可能
- **秒間レートリミット** は必須。AIエージェントはループする
- Tool Annotations でエージェントに安全性を伝える
- `.well-known/mcp.json` でDiscovery対応しておくと将来のエージェント自動発見に備えられる

AI Visibility IndexのMCPサーバーは **MCP公式レジストリに登録済み**、本番稼働中。Claude DesktopやCursorから自由に接続できる。

👉 [MCPサーバー](https://ai-visibility-index.dev/mcp)
👉 [MCP公式レジストリ](https://registry.modelcontextprotocol.io/servers/io.github.pulse-digital-dev/mcp-ai-visibility-index)
👉 [ソースコード（GitHub）](https://github.com/pulse-digital-dev/mcp-ai-visibility-index)
👉 [全104社のランキング](https://ai-visibility-index.dev/)
👉 [無料AI可視性チェック](https://ai-visibility-index.dev/tools/)

---

*このデータは[AI Visibility Index](https://ai-visibility-index.dev/)の独自調査。計測方法の詳細は[methodology](https://ai-visibility-index.dev/methodology/)を参照。*
