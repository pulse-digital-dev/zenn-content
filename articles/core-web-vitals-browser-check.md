---
title: "Core Web Vitalsをブラウザだけで確認する3つの方法——DevTools・PSI・Chrome拡張の使い分け"
emoji: "⚡"
type: "tech"
topics: ["webperformance", "CoreWebVitals", "lighthouse", "chrome"]
published: true
---

## はじめに

Core Web Vitals（CWV）は、Googleが検索ランキングの評価要素として使う**ユーザー体験の3指標**だ。2021年の導入以降、SEOの文脈で語られることが増えたが、「具体的にどう確認すればいいか」まで踏み込んだ解説は意外と少ない。

この記事では、追加ツールのインストールなしでブラウザだけで確認できる方法と、Chrome拡張を使った効率的な方法を、技術者向けに整理する。

## CWVの3指標と閾値

まず前提として、現行の3指標と「良好」判定の基準値を確認する。

| 指標 | 正式名称 | 何を測るか | 良好 | 要改善 | 不良 |
|------|----------|-----------|------|--------|------|
| LCP | Largest Contentful Paint | ページの読み込み速度 | ≤ 2.5秒 | ≤ 4.0秒 | > 4.0秒 |
| INP | Interaction to Next Paint | ユーザー操作への応答性 | ≤ 200ms | ≤ 500ms | > 500ms |
| CLS | Cumulative Layout Shift | 視覚的安定性 | ≤ 0.1 | ≤ 0.25 | > 0.25 |

出典: [web.dev — Web Vitals](https://web.dev/articles/vitals)

**INPについて:** 2024年3月にFID（First Input Delay）を正式に置き換えた指標。FIDが「最初の操作だけ」を測っていたのに対し、INPはページ滞在中の**すべての操作**の応答性を測定する。古い記事でFIDに言及しているものは、INPに読み替える必要がある。

Googleはこれを**実際のユーザーデータの75パーセンタイル**で評価している。つまり、訪問者の75%以上が「良好」の基準を満たしている必要がある。

## 方法①: Chrome DevTools（Lighthouse）

最も手軽な方法。追加インストールは一切不要。

### 手順

1. 確認したいページをChromeで開く
2. `F12`（Macは `⌘ + Option + I`）でDevToolsを開く
3. 上部タブから **「Lighthouse」** を選択
4. カテゴリで **「Performance」** にチェック
5. デバイスを「Mobile」または「Desktop」から選択
6. **「Analyze page load」** をクリック
7. 30秒〜1分ほどで結果が表示される

### 結果の読み方

Lighthouseレポートの上部にパフォーマンススコア（0〜100）が表示される。その下に各指標の値が信号色で表示される。

- **緑** — 良好。基準を満たしている
- **オレンジ** — 改善が必要
- **赤** — 不良。早急な改善が必要

### 技術的な注意点

Lighthouseは**ラボデータ（Lab data）**を使用する。シミュレーション環境での測定であり、実際のユーザー環境とは乖離が生じる。

特にINPはユーザーの操作が必要なため、Lighthouseでは直接測定できない。代わりに**TBT（Total Blocking Time）**で近似する。TBTが良好ならINPも概ね良好だが、逆は必ずしも成り立たない。

```
# DevToolsコンソールでCWVをリアルタイム取得するスニペット
# web-vitals ライブラリを動的にロードして計測する例

const script = document.createElement('script');
script.src = 'https://unpkg.com/web-vitals@4/dist/web-vitals.iife.js';
script.onload = () => {
  webVitals.onLCP(console.log);
  webVitals.onINP(console.log);
  webVitals.onCLS(console.log);
};
document.head.appendChild(script);
```

このスニペットをDevToolsのConsoleに貼り付けると、ページ操作中にCWV値がリアルタイムでログに出力される。Lighthouseを回さずにサッと確認したいときに便利。

## 方法②: PageSpeed Insights（PSI）

Googleが提供する公式Webツール。**フィールドデータ（実ユーザーデータ）とラボデータの両方**を確認できるのが最大の利点。

### 手順

1. [pagespeed.web.dev](https://pagespeed.web.dev/) にアクセス
2. 確認したいURLを入力
3. **「分析」** をクリック
4. 上部「実際のユーザーの環境で評価する」セクションでCWVのフィールドデータを確認
5. 下部「パフォーマンスの問題を診断する」セクションでLighthouse診断を確認

### フィールドデータ vs ラボデータ

| 種別 | データソース | 特徴 |
|------|-------------|------|
| フィールドデータ | CrUX（Chrome User Experience Report） | 実際のChromeユーザーから匿名収集。**Googleの検索ランキング評価に使われるのはこちら** |
| ラボデータ | Lighthouse | シミュレーション環境。再現性が高く開発時のデバッグに有用 |

**重要:** Googleの検索ランキングに影響するのはフィールドデータのみ。ラボデータでスコア100を出してもフィールドデータが不良なら、検索順位には反映されない。

### PSI APIでの自動化

定期的にチェックする場合は、PSI APIを使った自動化も可能。

```bash
# PSI API でモバイルのCWVを取得（APIキー不要、レート制限あり）
curl -s "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=https://example.com&strategy=mobile&category=PERFORMANCE" \
  | jq '.loadingExperience.metrics | {
    LCP: .LARGEST_CONTENTFUL_PAINT_MS.percentile,
    INP: .INTERACTION_TO_NEXT_PAINT.percentile,
    CLS: .CUMULATIVE_LAYOUT_SHIFT_SCORE.percentile
  }'
```

APIキーなしでも利用可能だが、レート制限がある。継続的な監視にはAPIキーを取得したほうがよい。

### フィールドデータが表示されない場合

フィールドデータは、CrUXに十分なトラフィックデータが蓄積されている場合にのみ表示される。新規サイトや低トラフィックのサイトでは「データが不十分です」となる。その場合はラボデータを参考にしつつ、トラフィックが増えるのを待つ。

## 方法③: Chrome拡張を使う

複数ページを日常的にチェックする場合は、Chrome拡張がもっとも効率的。

### CWV確認に使えるChrome拡張

| 拡張 | 開発元 | 特徴 |
|------|--------|------|
| Web Vitals | Google | CWV 3指標をリアルタイム表示。ブラウジング中に常時確認可能 |
| PagePulse | Pulse Digital | CWV関連チェック（画像最適化・render-blocking検出等）に加え、SEO・アクセシビリティ・構造を一括診断 |

**Web Vitals拡張**はページを開くだけでLCP・INP・CLSの値がバッジにリアルタイム表示される。ページ遷移のたびにリセットされるため、ページ単位での確認に向いている。

**PagePulse**はCWVの「原因」の診断に向いている。以下の項目を一括チェックできる。

- 画像に `width` / `height` 属性が設定されているか（CLS対策）
- render-blockingなスクリプトがないか（LCP対策）
- 画像の `loading="lazy"` が適切か（LCP対策）
- `<img>` タグにalt属性があるか（アクセシビリティ）

## ECサイト特有のCWV課題

ECサイトはCWVの改善が特に難しいジャンルの一つ。商品画像の多さ、外部スクリプトの重さなど、パフォーマンスに悪影響を与える要素が構造的に多い。

### よくある問題パターン

**LCPの悪化:**
ファーストビューの商品メイン画像に `loading="lazy"` が設定されているケース。テンプレートで全画像に一括適用されているプラットフォームもある。

```html
<!-- ❌ ファーストビューの画像にlazy loadingを付けてはいけない -->
<img src="product-main.jpg" loading="lazy" alt="商品名">

<!-- ✅ ファーストビューの画像はeagerまたは属性省略 -->
<img src="product-main.jpg" width="600" height="600" alt="商品名">

<!-- ✅ ファーストビュー外の画像にはlazy loadingを付ける -->
<img src="related-product.jpg" loading="lazy"
     width="300" height="300" alt="関連商品">
```

**CLSの悪化:**
商品画像に `width` / `height` 属性がなく、読み込み完了時にレイアウトがズレる。カルーセル（スライダー）の高さが動的に変わるケースも多い。

**INPの悪化:**
決済系・チャットウィジェット・広告タグなどのサードパーティスクリプトがメインスレッドをブロックしている。

### 改善の優先順位

Googleの[公式ガイド](https://web.dev/articles/top-cwv)を参考に、効果の大きい順に取り組むのが合理的。

1. **LCP** — ファーストビュー画像のlazy loading除外 + 次世代フォーマット（WebP/AVIF）変換
2. **CLS** — すべての `<img>` に `width` と `height` 属性を指定
3. **INP** — サードパーティスクリプトに `async` / `defer` 属性を付与

## 3つの方法の使い分け

| 目的 | おすすめ | 理由 |
|------|---------|------|
| 今すぐ1回だけ確認 | DevTools Lighthouse | インストール不要 |
| 実ユーザーデータを確認 | PageSpeed Insights | CrUXの実データ + 改善提案 |
| 日常的に複数ページ確認 | Chrome拡張 | ページ遷移ごとにリアルタイム確認可能 |

CWVは「改善すればすぐ検索順位が上がる」銀の弾丸ではない。ただし、ユーザー体験の品質を客観的に測る指標として、現状を把握しておくことには大きな価値がある。

---

**PagePulse** — CWV関連チェック + SEO・アクセシビリティ・構造を一括診断
🔗 https://chromewebstore.google.com/detail/pagepulse-ai-website-anal/biihgmkmmdihocjmdfedmhmkdmepfcoc

**関連記事:**
📝 ブログ完全版（CWV確認手順の詳細）: https://pulse-digital.dev/blog/core-web-vitals-check/
📝 ECプラットフォームのフロントエンド構造比較: https://pulse-digital.dev/blog/ec-platform-frontend-comparison/
📝 ECサイトに多い構造的な問題 5選: https://pulse-digital.dev/blog/ec-site-structure-issues/
