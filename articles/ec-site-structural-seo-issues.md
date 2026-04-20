---
title: "ECサイトの構造的なSEOエラー5選 ── 検出から修正コードまで"
emoji: "🛒"
type: "tech"
topics: ["ec", "seo", "html", "構造化データ", "フロントエンド"]
published: true
---

## はじめに

ECサイトの売上改善というと、デザインの刷新や広告予算の話になりがちだ。

だが実際は、HTMLの構造的な問題がSEO評価を下げ、SNSシェアを殺し、モバイルユーザーを離脱させているケースが驚くほど多い。厄介なのは、ブラウザが多少のエラーを補完してしまうので**見た目では気づけない**こと。

この記事では、ECサイトのフロントエンド構造を分析してきた中で**特に検出頻度が高かった5つの問題**と、それぞれの**コピペで使える修正コード**を書く。

対象読者: ECサイトのフロントエンドを触るエンジニア、テンプレートをカスタマイズする運営者。

---

## 1. Product構造化データの欠落・不備

**影響度: 高（リッチリザルト表示の有無に直結）**

Google検索結果で「価格」「在庫」「レビュー」がリッチリザルトとして表示されるかどうかは、`schema.org/Product` の構造化データで決まる。

### 検出パターン

- **Product自体が未実装** — MakeShopやカラーミーのデフォルトテンプレートでは未対応のケースがある
- **priceに通貨記号を含めている** — `"price": "¥3,980"` はNG。数値のみが正しい
- **availabilityの放置** — 在庫切れ商品でも `InStock` のまま

### 修正コード

商品ページの `<head>` 内に以下のJSON-LDを追加する。

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "商品名",
  "image": "https://example.com/product.jpg",
  "description": "商品の説明文",
  "brand": {
    "@type": "Brand",
    "name": "ブランド名"
  },
  "offers": {
    "@type": "Offer",
    "url": "https://example.com/product/123",
    "priceCurrency": "JPY",
    "price": "3980",
    "availability": "https://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "ショップ名"
    }
  }
}
</script>
```

**注意点:**
- `price` は数値のみ（カンマ・通貨記号なし）
- `availability` は在庫状態に連動して `InStock` / `OutOfStock` を動的に切り替える
- 検証: [リッチリザルトテスト](https://search.google.com/test/rich-results) で必ず確認

### 動的生成のヒント（PHPの場合）

```php
<?php
$product_data = [
    '@context' => 'https://schema.org',
    '@type' => 'Product',
    'name' => esc_html($product_name),
    'offers' => [
        '@type' => 'Offer',
        'priceCurrency' => 'JPY',
        'price' => preg_replace('/[^0-9]/', '', $price), // 数値のみに正規化
        'availability' => $in_stock
            ? 'https://schema.org/InStock'
            : 'https://schema.org/OutOfStock',
    ],
];
?>
<script type="application/ld+json">
<?php echo json_encode($product_data, JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES); ?>
</script>
```

---

## 2. og:image未設定によるSNSシェア機会の損失

**影響度: 高（SNSからの流入に直結）**

X（旧Twitter）やFacebookで商品ページがシェアされた際、`og:image` が未設定だとサムネイルが表示されない。テキストだけのリンクはCTRが大幅に落ちる。

ECサイトでは商品画像がそのままサムネイルになるので、設定するだけで無料の広告素材を手に入れるようなものだ。

### 修正コード

```html
<!-- 全商品ページの <head> 内に追加 -->
<meta property="og:title" content="商品名 | ショップ名">
<meta property="og:description" content="商品の簡潔な説明文">
<meta property="og:image" content="https://example.com/images/product-og.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:url" content="https://example.com/product/123">
<meta property="og:type" content="product">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="商品名 | ショップ名">
<meta name="twitter:description" content="商品の簡潔な説明文">
<meta name="twitter:image" content="https://example.com/images/product-og.jpg">
```

**画像の推奨仕様:**
- サイズ: **1200×630px**（各SNSのプレビューに最適化）
- 形式: JPEG or PNG（WebPは一部SNSで非対応の場合あり）
- ファイルサイズ: 1MB以下

### 検証方法

- X: [Card Validator](https://cards-dev.twitter.com/validator)（要ログイン）
- Facebook: [Sharing Debugger](https://developers.facebook.com/tools/debug/)

---

## 3. モバイルビューポートの設定不備

**影響度: 高（モバイルユーザー体験に直結）**

ECサイトのトラフィックの**60〜70%がモバイル経由**（経産省「令和6年度EC市場調査」で物販系61.7%）にもかかわらず、ビューポートメタタグの設定が不適切なサイトがある。

### 検出パターン

- **viewport自体が未設定** — デスクトップ表示がモバイルに縮小表示される
- **`maximum-scale=1` の固定** — ピンチズームが無効化され、WCAG 1.4.4（Resize Text）/ 1.4.10（Reflow）に違反

### 正しい設定

```html
<!-- ✅ 推奨 -->
<meta name="viewport" content="width=device-width, initial-scale=1">

<!-- ❌ NG: ズーム制限 -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

`maximum-scale=1` や `user-scalable=no` は削除する。視力に不安のあるユーザーがピンチズームできなくなるのはアクセシビリティ上の問題であり、Googleのモバイルフレンドリーテストでも警告対象になる。

### CSSの補完

```css
/* font-sizeをpx固定からremに変更 */
body { font-size: 1rem; }         /* 16px基準 */
h1   { font-size: 1.5rem; }      /* 24px */
.price { font-size: 1.125rem; }  /* 18px */

/* タッチターゲットの最小サイズ確保 */
button, a.btn, input[type="submit"] {
  min-height: 44px;
  min-width: 44px;
}
```

---

## 4. 画像alt属性の一括空白・重複

**影響度: 中（画像検索からの流入に影響）**

商品画像の `alt` 属性が空（`alt=""`）、またはすべて同じテキスト（例: 「商品画像」）に設定されているケース。

画像検索はECサイトにとって貴重な流入経路だ。適切な `alt` は検索エンジンに画像の内容を伝える唯一の手段。

### 修正パターン

```html
<!-- ❌ NG -->
<img src="product1.jpg" alt="">
<img src="product1-side.jpg" alt="商品画像">
<img src="product1-back.jpg" alt="商品画像">

<!-- ✅ OK -->
<img src="product1.jpg" alt="ブランドA スニーカー モデルX ホワイト 正面">
<img src="product1-side.jpg" alt="ブランドA スニーカー モデルX サイドビュー">
<img src="product1-back.jpg" alt="ブランドA スニーカー モデルX 背面ソール部分">
```

**alt属性の設置ルール:**
- メイン画像: 「ブランド名 + 商品名 + 型番」
- サブ画像: 角度や使用シーンを記述
- 装飾画像（区切り線、背景パターン）: `alt=""` でOK（スクリーンリーダーに読ませない）

---

## 5. canonicalタグの設定ミスによる重複コンテンツ

**影響度: 中（SEO評価シグナルの分散）**

ECサイトでは、同一商品がカラーバリエーション・サイズ別・ソート順別など複数のURLで表示される。`rel="canonical"` の設定が抜けていると、Googleが各URLを別ページとして扱い、SEO評価が分散する。

### 検出パターン

- パラメータ付きURL（`?color=red`、`?sort=price`）にcanonicalがない
- canonicalが自己参照ではなく、別ページを指している（設定ミス）
- httpとhttps、wwwあり/なしの混在

### 修正コード

```html
<!-- 全商品ページの <head> 内に自己参照canonicalを設定 -->
<link rel="canonical" href="https://example.com/products/123">
```

### パラメータ付きURLの正規化（PHP例）

```php
<?php
// クエリパラメータを除去した正規URLを生成
$canonical = strtok($_SERVER['REQUEST_URI'], '?');
$canonical = 'https://' . $_SERVER['HTTP_HOST'] . $canonical;
?>
<link rel="canonical" href="<?php echo esc_url($canonical); ?>">
```

### バリエーションの正規化方針

| パターン | canonical先 |
|---------|------------|
| `?color=red` `?color=blue` | パラメータなしURL |
| `/products/123/white` `/products/123/black` | 代表カラーのURL |
| `?sort=price` `?page=2` | パラメータなしURL（1ページ目） |

---

## まとめ

この5つは、いずれもHTMLやメタタグの修正で改善できる技術的課題だ。デザイン変更も広告予算も必要ない。

| # | 問題 | 影響度 | 修正工数目安 |
|---|------|--------|-------------|
| 1 | Product構造化データの欠落 | 高 | 30分〜2h |
| 2 | og:image未設定 | 高 | 15分〜1h |
| 3 | モバイルビューポート設定不備 | 高 | 5分〜30分 |
| 4 | 画像alt属性の空白・重複 | 中 | 商品数に依存 |
| 5 | canonical設定ミス | 中 | 30分〜1h |

効果は累積する。構造化データでリッチリザルトが表示されればCTRが上がり、og:imageでSNSシェアが増え、モバイル対応で直帰率が下がる。

まず自分のサイトの現状を数値で把握するところから始めるのがいい。

---

**PagePulse — この記事の5項目すべてをワンクリックで検出するChrome拡張**
🔗 https://chrome.google.com/webstore/detail/biihgmkmmdihocjmdfedmhmkdmepfcoc

**元記事（Pulse Digital Blog）**
🔗 https://pulse-digital.dev/blog/ec-site-structure-issues/
