---
title: "2026年6月 AIビジビリティランキング — 104社のECサイトを4つのAI APIで殴って引用パターンを調べた"
emoji: "🔬"
type: "tech"
topics: ["AI", "NLP", "API", "SEO", "データ分析"]
published: true
---

## やったこと

「ChatGPTに自社を推薦させる」ための技術的条件って何だろう？

これを調べるために、104社のECサイトに対して4つのAI API（OpenAI / Anthropic / Google Gemini / Perplexity）を叩き、引用パターンを統計分析した。

```javascript
// 計測の概要
const engines = ['chatgpt', 'claude', 'gemini', 'perplexity'];
const queries = 70;  // 業種別の購買系クエリ
const companies = 104;
// → 計 104 × 70 × 4 = 29120回のAPI呼び出し
```

毎月自動で回してデータを蓄積している。ソースコードと方法論は[こちら](https://ai-visibility-index.dev/methodology/)で公開中。

## 2026年6月の結果

| 指標 | 値 |
|------|----|
| 調査対象 | 104社（EC業界9カテゴリ） |
| 平均スコア | **1.18** / 100点 |
| 中央値 | 0.18点 |
| AIに見えてる企業 | **たった2社**（スコア20点以上） |

100点満点で平均1.2点。**104社中102社がAIにほぼ「見えていない」**。

## TOP 10

| # | 企業名 | 総合 | ChatGPT | Claude | Gemini | Perplexity |
|---|--------|------|---------|--------|--------|------------|
| 1 | Amazon Japan | 26.78 | 43.28 | 30.82 | 1.71 | 23.38 |
| 2 | 楽天市場 | 21.82 | 36.11 | 26.16 | 0.78 | 16.31 |
| 3 | Yahoo!ショッピング | 9.47 | 13.29 | 14.38 | 0.01 | 8.14 |
| 4 | ZOZOTOWN | 6.00 | 10.74 | 5.51 | 0.00 | 5.75 |
| 5 | メルカリ | 4.82 | 10.02 | 3.57 | 0.01 | 2.78 |
| 6 | ユニクロ | 4.13 | 5.90 | 7.15 | 0.39 | 1.19 |
| 7 | ビックカメラ.com | 3.81 | 5.67 | 4.62 | 0.39 | 3.79 |
| 8 | オイシックス | 3.78 | 6.88 | 4.08 | 0.44 | 1.61 |
| 9 | ヨドバシ.com | 3.72 | 4.09 | 5.26 | 0.39 | 5.82 |
| 10 | ニトリ | 3.55 | 6.67 | 2.95 | 0.55 | 2.24 |

## エンジン間の引用格差がえぐい

同じ企業でも、どのAIに聞くかでスコアが全然違う。

| エンジン | 全社平均 | 傾向 |
|---------|---------|------|
| ChatGPT | 1.80 | DA60+のサイトがスコア上位に集中 |
| Claude | 1.39 | FAQとschema.orgが効いてる |
| Gemini | **0.08** | ほぼ全社0.5pt未満。GBP完備のサイトだけスコアが付く |
| Perplexity | 1.25 | 直近30日の更新ページが引用される |

ChatGPTとGeminiの格差は**22.5倍**。GeminiはGBP・CWV・YouTubeのシグナルだけ見てる。

## 業界別

| 業界 | 社数 | 平均スコア | 最高スコア |
|------|------|-----------|-----------|
| EC総合 | 12社 | 5.98 | 26.78 |
| ファッション | 14社 | 1.26 | 6.00 |
| インテリア・家具 | 10社 | 0.81 | 3.55 |
| 家電・ガジェット | 12社 | 0.76 | 3.81 |
| 食品・グルメ | 12社 | 0.59 | 3.78 |

EC総合（Amazon, 楽天）が突出しているのは当然として、他の業界は壊滅的。

## AIに引用されるサイトの技術的条件

データを見ていると、スコアが高い企業にはいくつかの技術的共通点がある。

### 1. 構造化データ（JSON-LD）の実装

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "あなたの会社名",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "sameAs": [
    "https://twitter.com/yourcompany",
    "https://www.facebook.com/yourcompany"
  ],
  "description": "事業内容を簡潔に"
}
</script>
```

### 2. FAQスキーマの設置

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "おすすめの〇〇は？",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "△△です。理由は〜"
    }
  }]
}
</script>
```

FAQをJSON-LDで書いてるサイトは、ChatGPTとClaudeでスコアが高い。

### 3. dateModifiedの明示

```html
<meta property="article:modified_time" content="2026-06-01T00:00:00+09:00">
```

PerplexityはリアルタイムWeb検索なので、最終更新日が古いページは引用されにくい。

## 自社のスコアを確認する

全ランキングとスコアチェッカーを公開しているので、自社がどの程度AIに見えているか確認してみてほしい。

👉 [無料AI可視性チェック](https://ai-visibility-index.dev/tools/)
👉 [全104社のランキング](https://ai-visibility-index.dev/)

---

*本データは[AI Visibility Index](https://ai-visibility-index.dev/)の独自調査。計測方法の詳細は[methodology](https://ai-visibility-index.dev/methodology/)を参照。*
