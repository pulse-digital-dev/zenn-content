---
title: "Manifest V3でChrome拡張を作ってCWSに公開するまで"
emoji: "🔧"
type: "tech"
topics: ["chrome拡張", "manifestv3", "javascript", "個人開発"]
published: true
---

## はじめに

Chrome拡張を作ったことはあるだろうか。

作ったことがなければ、一度作ってみるといい。思ったより簡単で、思ったより実用的なものが短期間で形になる。

この記事では、Manifest V3準拠のChrome拡張を企画から Chrome Web Store 公開まで持っていった過程を、技術的な観点で書く。フレームワークなし、バンドラなし、Vanilla JSのみ。

作ったのは **PagePulse** という、Webページの品質をワンクリックで診断するツール。

## Manifest V3 の基本構造

Manifest V3（MV3）はChrome拡張の現行仕様。MV2は2024年にサポート終了しているので、今から作るなら必ずMV3で。

最小構成はこう。

```json
{
  "manifest_version": 3,
  "name": "My Extension",
  "version": "1.0.0",
  "description": "A minimal Chrome extension",
  "action": {
    "default_popup": "popup/popup.html",
    "default_icon": {
      "16": "icons/icon16.png",
      "48": "icons/icon48.png",
      "128": "icons/icon128.png"
    }
  },
  "permissions": ["activeTab"],
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content/analyzer.js"]
    }
  ]
}
```

### MV2→MV3での主な変更点

- `background.scripts` → `background.service_worker`（バックグラウンドページ廃止）
- `browser_action` / `page_action` → `action` に統合
- リモートコード実行の禁止（CDNからのスクリプト読み込みNG）
- `chrome.browserAction` → `chrome.action`

Service Worker化が一番大きい変更。常時起動ではなくイベント駆動になるので、状態を保持したい場合は `chrome.storage` を使う。

## アーキテクチャ設計

PagePulseの構成はこれだけ。

```
pagepulse/
├── manifest.json
├── background.js          # Service Worker
├── popup/
│   ├── popup.html
│   ├── popup.css
│   └── popup.js           # UIコントローラー
├── content/
│   └── analyzer.js        # ページ分析ロジック
└── icons/
```

ファイル数は7つ。コード全体で700行を切っている。

**なぜフレームワークを使わなかったか**

Chrome拡張のポップアップは、ブラウザのツールバーから開く小さなパネル。ReactやVueを入れるとバンドルサイズが数MBになり、ポップアップを開くたびにフレームワークの初期化コストが発生する。

今回は30項目程度の分析結果を表示するだけ。DOM操作もそこまで複雑ではない。Vanilla JSで `createElement` と `classList` を使えば十分だった。

## Content Script: ページ分析ロジック

心臓部は `content/analyzer.js`。Content Scriptとしてページに注入され、DOMを直接解析する。

### 分析の基本パターン

```javascript
function checkTitle() {
  const title = document.querySelector('title');
  
  if (!title || !title.textContent.trim()) {
    return { status: 'fail', message: 'Title tag is missing or empty' };
  }
  
  const length = title.textContent.trim().length;
  if (length < 30) {
    return { status: 'warn', message: `Title is short (${length} chars). Recommended: 30-60` };
  }
  if (length > 60) {
    return { status: 'warn', message: `Title is long (${length} chars). May be truncated in SERPs` };
  }
  
  return { status: 'pass', message: `Title: "${title.textContent.trim()}" (${length} chars)` };
}
```

すべてのチェック関数は `{ status, message }` を返す統一インターフェース。statusは `pass` / `fail` / `warn` / `info` の4種類。

### スコア算出

```javascript
function calculateCategoryScore(results) {
  const scorable = results.filter(r => r.status !== 'info');
  if (scorable.length === 0) return 100;
  
  let score = 100;
  const penalty = 100 / scorable.length;
  
  scorable.forEach(r => {
    if (r.status === 'fail') score -= penalty;
    if (r.status === 'warn') score -= penalty * 0.5;
  });
  
  return Math.max(0, Math.round(score));
}
```

カテゴリスコアは100点からの減点方式。failは1項目あたりの満点分を丸ごと引く。warnは半分。infoはスコアに影響しない。

総合スコアはカテゴリごとの加重平均。

```javascript
const weights = { seo: 0.20, performance: 0.20, accessibility: 0.15, structure: 0.15, llmo: 0.30 };
```

LLMOの比重を30%にしている理由は、「まだ誰も対策してない領域で差がつきやすい」から。

## Popup: UI表示

`popup.js` が受け取った分析結果をレンダリングする。

### Content Scriptとの通信

```javascript
// popup.js
chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
  chrome.tabs.sendMessage(tabs[0].id, { action: 'analyze' }, (response) => {
    if (response && response.results) {
      renderResults(response.results);
    }
  });
});
```

```javascript
// analyzer.js (Content Script側)
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'analyze') {
    const results = runFullAnalysis();
    sendResponse({ results });
  }
  return true; // 非同期レスポンスのため
});
```

MV3では `chrome.runtime.onMessage` のコールバックで `return true` を返さないと、非同期レスポンスが途切れる。ここでハマった。

### CSS: ダークモード + Glassmorphism

```css
:root {
  --bg-primary: #1a1a2e;
  --bg-card: rgba(255, 255, 255, 0.05);
  --border: rgba(255, 255, 255, 0.1);
  --text-primary: #e8e8e8;
  --success: #00c896;
  --warning: #ffb800;
  --error: #ff4757;
}

.score-card {
  background: var(--bg-card);
  backdrop-filter: blur(10px);
  border: 1px solid var(--border);
  border-radius: 12px;
}
```

Chrome拡張のポップアップは基本的にダークなUIが映える。Glassmorphism（半透明 + blur）を控えめに使うと、情報密度が高い画面でも圧迫感が減る。

## エクスポート機能

分析結果をCSVとJSONで出力できる。

```javascript
function exportToCSV(results) {
  const headers = ['Category', 'Check', 'Status', 'Message'];
  const rows = [];
  
  Object.entries(results).forEach(([category, items]) => {
    items.forEach(item => {
      rows.push([category, item.name, item.status, item.message]);
    });
  });
  
  const csv = [headers, ...rows]
    .map(row => row.map(cell => `"${cell}"`).join(','))
    .join('\n');
  
  const blob = new Blob([csv], { type: 'text/csv' });
  const url = URL.createObjectURL(blob);
  
  chrome.downloads.download({
    url,
    filename: `pagepulse-report-${Date.now()}.csv`
  });
}
```

MV3では `chrome.downloads` APIを使う場合、`permissions` に `"downloads"` を追加する必要がある。ただしPagePulseは `Blob` + `URL.createObjectURL` + `<a>` タグのダウンロードトリックを使っているので、downloads権限は不要。

## Chrome Web Store 申請で気をつけること

### 1. 権限は最小限に

`<all_urls>` を使うと審査が厳しくなる。理由の記載が求められる。

PagePulseの場合、Content Scriptをすべてのページで実行する必要があるため `<all_urls>` は必須。ただし審査時に「なぜこの権限が必要か」を明確に説明すればリジェクトは避けられた。

`storage` 権限を追加した場合も、「プライバシーへの取り組み」タブで理由を記載する必要がある。「ユーザーの設定情報をローカルに保存するため」のような具体的な説明を書く。

### 2. スクリーンショットは先に用意

申請時に1280×800のスクリーンショットが最低1枚必要。最大5枚。

いざ申請しようとしたときに「スクショ撮ってない」と気づくとバタバタする。開発中からキャプチャしておくのがおすすめ。

### 3. description は英語で

CWSは全世界に公開されるので、description は英語で書いたほうがリーチが広がる。日本語サポートが必要な場合は、ローカライズ設定（`_locales/`）を使う。

### 4. 審査期間

公式には「数日」とされているが、自分の場合は2-3営業日で通った。権限が少ないシンプルな拡張なら早い傾向にある。

## まとめ

Chrome拡張開発のハードルは、思ったより低い。

- Manifest V3の基本構造を理解する（1時間）
- Content Script + Popup の通信パターンを覚える（1時間）
- あとは普通のJS/CSS/HTMLを書くだけ

フレームワークなしでも問題ない規模のツールは多い。「小さく作って素早く公開」の精神で、まずは動くものを出す方が良い結果につながる。

公開後の改善は、実ユーザーのフィードバックに基づいて進めればいい。

---

**PagePulse — Chrome Web Store で無料公開中**
🔗 https://chrome.google.com/webstore/detail/biihgmkmmdihocjmdfedmhmkdmepfcoc
