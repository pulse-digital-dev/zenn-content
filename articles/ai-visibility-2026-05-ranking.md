---
title: "2026年5月 AIビジビリティランキング — 104社のECサイトを4つのAI APIで殴って引用パターンを調べた"
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

## 2026年5月の結果

| 指標 | 値 |
|------|----|
| 調査対象 | 104社（EC業界9カテゴリ） |
| 平均スコア | **1.27** / 100点 |
| 中央値 | 0.27点 |
| AIに見えてる企業 | **たった2社**（スコア20点以上） |

100点満点で平均1.3点。**104社中102社がAIにほぼ「見えていない」**。

## TOP 10

| # | 企業名 | 総合 | ChatGPT | Claude | Gemini | Perplexity |
|---|--------|------|---------|--------|--------|------------|
| 1 | Amazon Japan | 28.07 | 42.04 | 30.90 | 13.18 | 15.58 |
| 2 | 楽天市場 | 23.15 | 35.46 | 26.24 | 7.76 | 14.91 |
| 3 | Yahoo!ショッピング | 10.46 | 13.46 | 14.19 | 3.85 | 8.25 |
| 4 | ZOZOTOWN | 6.15 | 10.52 | 5.39 | 2.04 | 4.08 |
| 5 | メルカリ | 5.58 | 11.00 | 3.86 | 1.03 | 3.41 |
| 6 | ユニクロ | 4.12 | 5.65 | 7.11 | 0.66 | 1.33 |
| 7 | オイシックス | 3.97 | 6.27 | 4.04 | 1.37 | 2.84 |
| 8 | ヨドバシ.com | 3.93 | 4.07 | 5.28 | 1.86 | 4.83 |
| 9 | ビックカメラ.com | 3.93 | 5.80 | 4.74 | 0.96 | 3.14 |
| 10 | ニトリ | 3.79 | 7.05 | 2.76 | 1.28 | 2.12 |

## エンジン間の引用格差がえぐい

同じ企業でも、どのAIに聞くかでスコアが全然違う。

| エンジン | 全社平均 | 傾向 |
|---------|---------|------|
| ChatGPT | 1.79 | ドメイン権威性（DA）で引用先を選んでる感 |
| Claude | 1.38 | 情報品質とFAQの充実度を見てる |
| Gemini | **0.56** | 最も門戸が狭い。ほぼ全社0.5pt未満 |
| Perplexity | 1.07 | リアルタイム検索。更新頻度が効く |

ChatGPTとGeminiの格差は**3.2倍**。Geminiは自社のGoogle生態系（GBP、CWV、YouTube）のシグナルしか見ていない可能性が高い。

## 業界別

| 業界 | 社数 | 平均スコア | 最高スコア |
|------|------|-----------|-----------|
| EC総合 | 12社 | 6.44 | 28.07 |
| ファッション | 14社 | 1.26 | 6.15 |
| インテリア・家具 | 10社 | 0.85 | 3.79 |
| 家電・ガジェット | 12社 | 0.81 | 3.93 |
| 食品・グルメ | 12社 | 0.64 | 3.97 |

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

FAQをJSON-LDで書くと、ChatGPTとClaudeのスコアが上がりやすい傾向が見える。

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
