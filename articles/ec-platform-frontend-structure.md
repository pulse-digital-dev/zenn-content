---
title: "Shopify・MakeShop・カラーミーのフロントエンド構造を技術比較する"
emoji: "🏗️"
type: "tech"
topics: ["ec", "shopify", "seo", "html"]
published: true
---

## はじめに

ECプラットフォームの比較記事は星の数ほどある。だがその大半は管理画面の機能や月額料金の比較で、**出力されるHTMLの構造**を技術的に比較しているものは少ない。

この記事では、Shopify・MakeShop・カラーミーショップの3プラットフォームについて、**テンプレートエンジン・構造化データ・SEOタグ・画像最適化・レスポンシブ対応**の5軸でフロントエンド構造を比較する。

筆者はStorePulse（ECサイト向け診断Chrome拡張）のプラットフォーム自動検出エンジンを開発する過程で、多数のECプラットフォームのHTML出力を解析してきた。その知見を整理したものがこの記事となる。

なお、比較情報は公式ドキュメントに基づく。公式ドキュメントに記載がない項目は「不明」として扱い、推測では埋めていない。

## 1. テンプレートエンジンの設計

テンプレートエンジンの設計は、プラットフォームの拡張性を決める最大の要素だ。

| 項目 | Shopify | MakeShop | カラーミー |
|------|---------|----------|-----------|
| テンプレート言語 | Liquid | Smarty | 独自タグ |
| ファイル拡張子 | `.liquid` | 不明 | 不明 |
| HTML編集自由度 | 高い。条件分岐・ループ等のロジックも記述可能 | クリエイターモードで可能。Smartyの一部組み込み関数に制約あり | **`<head>` と `<body>` の直接編集不可**という強い制約あり |

### Shopify: Liquid

Shopifyの`Liquid`はShopify社が開発したテンプレート言語。Rubyベースで、フィルターとタグによるロジック記述が可能。

```liquid
{% if product.available %}
  <span class="price">{{ product.price | money }}</span>
{% else %}
  <span class="sold-out">売り切れ</span>
{% endif %}
```

テーマのコード全体にアクセスでき、`<head>` 内のメタタグからフッターのスクリプトまで自由に編集できる。

### MakeShop: Smarty

MakeShopはPHPのテンプレートエンジン `Smarty` を採用している。クリエイターモード（月額11,000円の上位プラン）でテンプレートの直接編集が可能になる。

ただし、Smartyの組み込み関数の一部に制約があり、Shopifyほどの自由度はない。

### カラーミーショップ: 独自タグ

カラーミーのテンプレートは独自の差し込みタグ方式。特に注意すべきは **`<head>`タグと`<body>`タグ自体を直接編集できない** という制約。構造化データやSEOメタタグを自由に追加するのが難しいケースがある。

## 2. 構造化データ（schema.org）のデフォルト対応

ECサイトでリッチリザルト（検索結果に価格・在庫を表示）を出すには、`Product` 構造化データが必要。プラットフォームのデフォルト対応状況を確認すると差が大きい。

| 項目 | Shopify | MakeShop | カラーミー |
|------|---------|----------|-----------|
| Product | **標準搭載**（Dawn等の公式テーマ） | **標準搭載**（Completeテンプレート） | **なし**（独自タグで手動追加が必要） |
| BreadcrumbList | あり（Dawn v15以降） | 不明 | 不明 |
| 形式 | JSON-LD推奨 | 不明 | JSON-LD推奨（手動設定時） |
| 自動出力される項目 | URL、offers、brand、review、GTIN等 | 商品レビュー（価格・在庫は手動追加が必要） | なし（全項目を手動で構築） |

### Shopify の構造化データ出力例

Shopifyの公式テーマDawnを使った場合、商品ページに自動出力される構造化データはこのような形になる。

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "商品名",
  "image": "https://cdn.shopify.com/.../product.jpg",
  "offers": {
    "@type": "Offer",
    "price": "3980",
    "priceCurrency": "JPY",
    "availability": "https://schema.org/InStock"
  }
}
```

Dawnテーマでは `templates/product.json` 内のセクション設定で自動出力される。テーマを変更している場合は出力されないことがある。

### カラーミーで構造化データを追加する場合

カラーミーでProduct構造化データを追加する場合は、`<head>` 直接編集の制約があるため、商品ページのフリースペースやテンプレートのbody内にJSON-LDスクリプトを手動で埋め込む方式になる。

```html
<!-- カラーミーの商品ページテンプレートに手動追加 -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "<{$product.name}>",
  "offers": {
    "@type": "Offer",
    "price": "<{$product.price}>",
    "priceCurrency": "JPY"
  }
}
</script>
```

JSON-LDは `<body>` 内に配置しても有効なので、`<head>` が編集できなくても対応可能。ただし独自タグの変数名を正確に把握する必要がある。

## 3. SEOデフォルト設定

| 項目 | Shopify | MakeShop | カラーミー |
|------|---------|----------|-----------|
| title / meta description | **自動出力**（`page_title` / `page_description`） | 不明 | 不明 |
| canonical | **自動出力**（UTMパラメータ等のURL正規化対応） | 不明 | 不明 |

Shopifyの`canonical`タグ自動出力は、ECサイトで頻発する「同一商品が複数URLに分散する問題」をプラットフォームレベルで予防する仕組みとして有効。

```liquid
{%- comment -%}
  Shopifyテーマ内でのcanonical自動出力（theme.liquid）
{%- endcomment -%}
<link rel="canonical" href="{{ canonical_url }}">
```

## 4. 画像最適化とCDN

| 項目 | Shopify | MakeShop | カラーミー |
|------|---------|----------|-----------|
| CDN | `cdn.shopify.com` 経由 | CDN利用（閲覧環境に応じた配信） | 画像変換プロキシ経由でキャッシュ配信 |
| 自動フォーマット変換 | 不明 | **WebP / AVIF自動変換**（2025年4月〜） | JPEG自動最適化（WebP変換は未対応） |

注目すべきはMakeShopが2025年4月にWebP/AVIF自動変換を導入した点。次世代画像フォーマットへの自動変換はCore Web Vitals（特にLCP）の改善に直結するため、パフォーマンス面での大きなアドバンテージとなる。

## 5. レスポンシブ対応方式

| プラットフォーム | 方式 | 備考 |
|-----------------|------|------|
| Shopify | 不明（公式ドキュメントに記載なし） | 公式テーマ（Dawn等）はレスポンシブデザインで設計 |
| MakeShop | **統合型（レスポンシブ）** | クリエイターモードで対応。旧ベーシックモードはPC/SP分離型 |
| カラーミー | **分離型** | PC / SP / 携帯でテンプレート変数が分離（`PC_IMAGES_PATH` / `SMART_IMAGES_PATH`） |

カラーミーのPC/SP分離型は、修正時に複数テンプレートの更新が必要になる。Googleはモバイルファーストインデックスでレスポンシブデザインを推奨しており、SEO観点では統合型が望ましい。

## まとめ

| 観点 | Shopify | MakeShop | カラーミー |
|------|---------|----------|-----------|
| テンプレート自由度 | ◎ Liquid | ○ Smarty（制約あり） | △ 独自タグ（head編集不可） |
| 構造化データ | ◎ 標準搭載 | ○ 部分的に標準搭載 | × なし（手動追加） |
| SEOタグ自動出力 | ◎ 充実 | 不明 | 不明 |
| 画像最適化 | ○ CDN | ◎ WebP/AVIF自動変換 | △ JPEG最適化のみ |
| レスポンシブ | ○ テーマ依存 | ○ 統合型（クリエイターモード） | △ 分離型 |

プラットフォーム選定では、料金や機能一覧だけでなく **「出力されるHTMLの構造」** を確認することを推奨する。テーマやプラグインの構成で実際の出力は変わるので、自サイトの現状を実際に確認するのが最も確実だ。

---

**StorePulse** — ECサイトのプラットフォーム自動検出 + 構造診断
🔗 https://chromewebstore.google.com/detail/ngcibjldjcdjcmfegcnklljoknicmjfl

**関連記事:**
📝 ブログ完全版（比較表・補足情報付き）: https://pulse-digital.dev/blog/ec-platform-frontend-comparison/
📝 ECサイトに多い構造的な問題 5選: https://pulse-digital.dev/blog/ec-site-structure-issues/
