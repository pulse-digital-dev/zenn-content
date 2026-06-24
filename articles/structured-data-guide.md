---
title: "構造化データでAI検索引用率を上げる完全ガイド — EC業種別JSON-LDテンプレート付き"
emoji: "🏗️"
type: "tech"
topics: ["構造化データ","JSONLD","SEO","AI","EC"]
published: true
---

*構造化データがWebサイトとAIエンジンを接続する概念図*


EC業界104社を調査した結果、構造化データ（JSON-LD）を正しく実装している企業のAI引用率は、未実装企業の1.4倍だった。しかし実装率はわずか23%。ここに、先行者利益を獲得するチャンスがある。


ChatGPTやGeminiは、Webページを人間のように読んでいるわけではない。HTMLの意味構造を解析し、構造化データを手がかりにして情報を理解する。つまり構造化データは、AI検索時代の「名刺」のようなものだ。名刺を持たずに商談に行く企業は信頼されない。構造化データなしにAI検索で引用されることも、また難しい。


NOTE


この記事はGEO（Generative Engine Optimization）の実践ガイドです。AI検索対策の全体像は[GEO/LLMO/AIO完全ガイド](https://ai-visibility-index.dev/blog/what-is-geo/)を、はじめての方は[GEO対策の始め方](https://ai-visibility-index.dev/blog/geo-getting-started/)もあわせてご覧ください。


## 104社データで見る構造化データとAI引用の相関


[AI Visibility Index](https://ai-visibility-index.dev/)の2026年5月度調査（EC業界104社、4大AIエンジン、170クエリ）から、構造化データがAI引用にどう影響するかを分析した。


:::message
**構造化データ実装状況と引用率の相関（104社）**
- **JSON-LD実装企業の平均スコア:** 未実装企業の1.4倍
- **OGPタグ実装企業の平均スコア:** 未実装企業の2.7倍
- **JSON-LD + OGP + FAQ 3点実装済み:** 104社中わずか12社（12%）
:::


### エンジン別に見る構造化データの効果


構造化データの影響度はAIエンジンごとに異なる。各エンジンの特性を理解した上で実装を進めることが重要だ。


| AIエンジン | 平均スコア | 構造化データへの反応 | 最も効くスキーマ |
| --- | --- | --- | --- |
| **ChatGPT** | 1.8点 | ブランド認知を重視。構造化データは補助的 | Organization + sameAs |
| **Claude** | 1.4点 | コンテンツの専門性を重視 | Article + FAQPage |
| **Perplexity** | 1.1点 | OGPタグに最も強く反応（3.6倍） | OGP + FAQPage |
| **Gemini** | 0.6点 | JSON-LD実装企業を2.0倍引用 | Product + FAQPage + HowTo |


特に注目すべきは**Gemini**だ。104社中92社がGeminiスコア1点未満という極めて保守的なエンジンだが、JSON-LD実装企業の引用率は未実装の2.0倍。構造化データが最も効果を発揮するエンジンでもある。Google AI Overview（旧SGE）対策を考えると、Geminiへの最適化は無視できない。詳しくは[AI Overview対策ガイド](https://ai-visibility-index.dev/blog/ai-overview-guide/)を参照。


## 構造化データとは


構造化データとは、Webページのコンテンツに対して**機械が理解できる形式で意味（セマンティクス）を付与する**ためのマークアップ技術である。


例えば、人間は「¥3,980」というテキストを見れば「価格だ」と直感的に理解できる。しかし検索エンジンやLLMにとっては、それが価格なのか、型番なのか、電話番号の一部なのかを文脈から推測する必要がある。構造化データは、この曖昧さを排除する。


構造化データは、ページの内容に関する情報を提供し、ページのコンテンツを分類するために標準化されたフォーマットです。


— Google Search Central ドキュメントより


## AI引用に効く主要Schema.orgタイプ


*Schema.orgの主要タイプ一覧 — Organization、Product、FAQ、Articleなどのカテゴリアイコン*


AI引用率の向上に特に効果が高いSchema.orgのタイプを、優先度順に整理した。


| タイプ | 用途 | AI引用への効果 | 対象 |
| --- | --- | --- | --- |
| Organization | 企業・組織情報 | 全エンジンの基盤。sameAsでWikipedia連携 | 全サイト必須 |
| Product + Offer | 商品情報 | ECサイトの引用率に直結 | ECサイト |
| FAQPage | よくある質問 | AIが直接引用しやすい形式 | 全サイト推奨 |
| Article | 記事・ブログ | 著者・日付の信頼性シグナル | メディア・ブログ |
| HowTo | 手順ガイド | Geminiが特に参照する傾向 | ハウツー系コンテンツ |
| BreadcrumbList | パンくずリスト | サイト構造の理解を補助 | 全サイト推奨 |
| Review / AggregateRating | レビュー・評価 | 社会的証明としてAIが参照 | EC・サービス |


TIP


まずは`Organization`と`FAQPage`から始めるのがおすすめです。実装が簡単で、AI引用への効果が最も高い組み合わせです。


## JSON-LDの基本構造


構造化データの記述フォーマットには**JSON-LD**（JavaScript Object Notation for Linked Data）を推奨する。Googleも公式に推奨しており、HTMLの``セクションに``タグとして配置するだけでよい。


基本テンプレート — Organization
&lt;script type="application/ld+json"&gt;
{
"@context": "https://schema.org",
"@type": "Organization",
"name": "あなたの会社名",
"url": "https://example.com",
"logo": "https://example.com/logo.png",
"description": "企業の説明文（AIが引用する際のソース）",
"foundingDate": "2020-01-01",
"sameAs": [
"https://twitter.com/example",
"https://www.facebook.com/example",
"https://ja.wikipedia.org/wiki/あなたの会社名"
],
"contactPoint": {
"@type": "ContactPoint",
"telephone": "+81-3-1234-5678",
"contactType": "customer service",
"availableLanguage": "Japanese"
},
"address": {
"@type": "PostalAddress",
"addressCountry": "JP",
"addressRegion": "東京都",
"addressLocality": "渋谷区",
"streetAddress": "道玄坂1-10-8"
}
}
&lt;/script&gt;this.classList.remove('copied'),2000)" aria-label="コードをコピー">


IMPORTANT


`sameAs`にWikipediaのURLを含めることは非常に重要です。104社調査では、Wikipedia掲載企業の引用率は未掲載企業の2.7倍でした。sameAsによるWikipedia連携は、AIに「この組織は信頼できる」と認識させる最も直接的な方法です。


## EC業種別 JSON-LDテンプレート集


以下のテンプレートはそのままコピーして自社の値に書き換えるだけで使える。業種ごとの最適なスキーマ構成を、104社調査の引用傾向データに基づいて設計した。


### ファッションEC — Product + Brand + AggregateRating


ファッション業界は平均スコア1.3点（104社中2位）と、総合ECに次いで高い。ZOZOTOWNの6.2点が業界を牽引するが、個別ブランドはほぼ0点台。ブランド情報の構造化が差別化の鍵になる。


fashion-product.html
{
"@context": "https://schema.org",
"@type": "Product",
"name": "オーガニックコットン オーバーサイズTシャツ",
"description": "GOTS認証オーガニックコットン100%。リラックスフィットのユニセックスT。",
"image": "https://example.com/images/tshirt-organic.jpg",
"sku": "TS-ORG-001",
"brand": {
"@type": "Brand",
"name": "あなたのブランド名"
},
"material": "オーガニックコットン100%",
"color": "ナチュラルホワイト",
"size": "S / M / L / XL",
"offers": {
"@type": "Offer",
"price": 4980,
"priceCurrency": "JPY",
"availability": "https://schema.org/InStock",
"priceValidUntil": "2027-03-31",
"url": "https://example.com/products/tshirt-organic",
"shippingDetails": {
"@type": "OfferShippingDetails",
"shippingRate": {
"@type": "MonetaryAmount",
"value": 0,
"currency": "JPY"
},
"shippingDestination": {
"@type": "DefinedRegion",
"addressCountry": "JP"
},
"deliveryTime": {
"@type": "ShippingDeliveryTime",
"businessDays": {
"@type": "QuantitativeValue",
"minValue": 1,
"maxValue": 3
}
}
}
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.6",
"reviewCount": "328"
}
}this.classList.remove('copied'),2000)" aria-label="コードをコピー">


### 食品EC — Product + NutritionInformation


食品業界は平均0.6点。オイシックス（4.0点）を除くと大半が0点台だ。食品特有の栄養情報をスキーマ化することで、「カロリーが低い菓子は？」「糖質制限に合うパンは？」といったAIへの質問で引用される確率が上がる。


food-product.html
{
"@context": "https://schema.org",
"@type": "Product",
"name": "有機抹茶ラテミックス 200g",
"description": "京都府産の有機抹茶を使用。お湯を注ぐだけで本格抹茶ラテが完成。",
"image": "https://example.com/images/matcha-latte.jpg",
"brand": {
"@type": "Brand",
"name": "あなたのブランド名"
},
"offers": {
"@type": "Offer",
"price": 1980,
"priceCurrency": "JPY",
"availability": "https://schema.org/InStock",
"priceValidUntil": "2027-03-31"
},
"nutrition": {
"@type": "NutritionInformation",
"calories": "45 kcal",
"servingSize": "1杯分（10g）",
"sugarContent": "3.2 g",
"proteinContent": "1.8 g"
},
"additionalProperty": [
{
"@type": "PropertyValue",
"name": "原産地",
"value": "京都府宇治市"
},
{
"@type": "PropertyValue",
"name": "認証",
"value": "有機JAS認証"
}
]
}this.classList.remove('copied'),2000)" aria-label="コードをコピー">


### コスメ・美容EC — Product + Review + ItemList


コスメ業界は平均0.4点と低迷するが、注目すべきはTHREE（0.7点）がGeminiスコア2.5点を記録している点だ。Gemini>=1ptの企業はわずか12社で、THREEはその1社。構造化データの実装品質が要因と推測される。


beauty-product.html
{
"@context": "https://schema.org",
"@type": "Product",
"name": "薬用美白美容液 30mL",
"description": "ビタミンC誘導体配合の医薬部外品。シミ・そばかすを防ぎ、透明感のある肌へ。",
"image": "https://example.com/images/serum-vc.jpg",
"brand": {
"@type": "Brand",
"name": "あなたのブランド名"
},
"category": "スキンケア > 美容液",
"offers": {
"@type": "Offer",
"price": 5980,
"priceCurrency": "JPY",
"availability": "https://schema.org/InStock",
"priceValidUntil": "2027-03-31"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.8",
"reviewCount": "512"
},
"review": [
{
"@type": "Review",
"author": { "@type": "Person", "name": "利用者A" },
"reviewRating": {
"@type": "Rating",
"ratingValue": "5"
},
"reviewBody": "1ヶ月使用でトーンアップを実感。べたつかずサラッとした使用感。"
}
]
}this.classList.remove('copied'),2000)" aria-label="コードをコピー">


### 家電EC — Product + AggregateRating + FAQ


家電業界は平均0.8点。ヨドバシ.com（3.9点）がPerplexity 4.8点とChatGPTを逆転する注目の事例だ。スペック情報のFAQ化がAI引用に効く。


electronics-product.html
[
{
"@context": "https://schema.org",
"@type": "Product",
"name": "ワイヤレスノイズキャンセリングヘッドホン XM5",
"description": "業界最高クラスのノイキャン性能。30時間連続再生。マルチポイント接続対応。",
"image": "https://example.com/images/headphone-xm5.jpg",
"brand": {
"@type": "Brand",
"name": "TechAudio"
},
"offers": {
"@type": "Offer",
"price": 39800,
"priceCurrency": "JPY",
"availability": "https://schema.org/InStock",
"priceValidUntil": "2027-03-31"
},
"aggregateRating": {
"@type": "AggregateRating",
"ratingValue": "4.7",
"reviewCount": "1284"
}
},
{
"@context": "https://schema.org",
"@type": "FAQPage",
"mainEntity": [
{
"@type": "Question",
"name": "XM5のバッテリー持続時間は？",
"acceptedAnswer": {
"@type": "Answer",
"text": "ノイズキャンセリングON時で約30時間、OFF時で約38時間の連続再生が可能です。急速充電にも対応しており、3分の充電で約1時間再生できます。"
}
},
{
"@type": "Question",
"name": "XM5は複数デバイスの同時接続に対応していますか？",
"acceptedAnswer": {
"@type": "Answer",
"text": "はい。マルチポイント接続に対応しており、スマートフォンとPCなど2台同時に接続できます。着信時は自動でスマートフォンに切り替わります。"
}
}
]
}
]this.classList.remove('copied'),2000)" aria-label="コードをコピー">


### サービス・D2C — LocalBusiness + Service + FAQ


D2C業界は技術シグナルの実装率がEC業界で最も高い（OGP実装率100%、サイトマップ実装率100%）にもかかわらず、平均スコアは0.3点と9業界中8位。これが「D2Cパラドックス」だ。構造化データだけでなく、ブランドメンション獲得と組み合わせる必要がある。


service-business.html
{
"@context": "https://schema.org",
"@type": "LocalBusiness",
"name": "あなたのブランド名",
"description": "オーダーメイドスーツのD2Cブランド。初回採寸から納品まで最短2週間。",
"url": "https://example.com",
"image": "https://example.com/images/store-front.jpg",
"priceRange": "¥¥",
"address": {
"@type": "PostalAddress",
"addressCountry": "JP",
"addressRegion": "東京都",
"addressLocality": "渋谷区"
},
"geo": {
"@type": "GeoCoordinates",
"latitude": 35.6580,
"longitude": 139.7016
},
"openingHoursSpecification": {
"@type": "OpeningHoursSpecification",
"dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
"opens": "10:00",
"closes": "19:00"
},
"hasOfferCatalog": {
"@type": "OfferCatalog",
"name": "サービス一覧",
"itemListElement": [
{
"@type": "Service",
"name": "オーダーメイドスーツ",
"description": "採寸から納品まで最短2週間。生地300種類以上から選択可能。",
"offers": {
"@type": "Offer",
"price": 49800,
"priceCurrency": "JPY"
}
}
]
}
}this.classList.remove('copied'),2000)" aria-label="コードをコピー">


## FAQPageスキーマの実装


FAQPageスキーマは、AIエンジンが直接回答として引用しやすい形式を提供する。104社調査では、FAQPage実装企業はGeminiで特に高い引用率を示した。実装のポイントは以下の3つだ。


1. **質問は具体的に書く** — 「どうやって使いますか？」ではなく「XM5ヘッドホンのBluetooth接続手順は？」のように具体的に。
2. **回答は完結させる** — AIが引用する際に、回答だけで意味が通じる文章にする。「詳しくはこちら」で終わらせない。
3. **定期的に更新する** — 古いFAQはAIの信頼スコアを下げる原因になる。最低でも四半期ごとに見直すこと。


faq-page.html
{
"@context": "https://schema.org",
"@type": "FAQPage",
"mainEntity": [
{
"@type": "Question",
"name": "構造化データはAI検索に効果がありますか？",
"acceptedAnswer": {
"@type": "Answer",
"text": "はい。EC業界104社の調査では、JSON-LD構造化データを実装している企業のAI引用率は未実装企業の1.4倍でした。特にGeminiではJSON-LD実装企業の引用率が2.0倍と、4エンジン中で最も大きな差が確認されています。"
}
},
{
"@type": "Question",
"name": "構造化データの実装にかかる時間は？",
"acceptedAnswer": {
"@type": "Answer",
"text": "Organization + FAQPage の基本実装であれば、1ページあたり30分程度で完了します。ECサイトの商品ページにProduct + Offerを一括実装する場合は、CMSのテンプレート修正が必要なため、開発リソースとして1-2日を見込んでください。"
}
},
{
"@type": "Question",
"name": "JSON-LDとMicrodataのどちらを使うべきですか？",
"acceptedAnswer": {
"@type": "Answer",
"text": "JSON-LDを推奨します。Googleも公式に推奨しており、HTMLのheadセクションにscriptタグとして配置するだけなので、既存のHTMLを変更する必要がありません。また、AIエンジンはJSON-LD形式のデータをより正確にパースできます。"
}
}
]
}this.classList.remove('copied'),2000)" aria-label="コードをコピー">


## テスト・検証の方法


*構造化データのテスト・検証ツールの概念図 — バリデーションパネルにチェックマークが並ぶイラスト*


構造化データの実装が正しいかを確認するには、以下のツールを活用する。


- **Google Rich Results Test** — リッチリザルトの対象かどうかを検証。URLまたはコードスニペットを直接入力可能。
- **Schema.org Validator** — 構文エラーや必須プロパティの欠落を検出。より厳密なバリデーション。
- **Google Search Console** — 実際にGoogleがインデックスした構造化データの状態を確認。エラー・警告の一覧表示。
- **Chrome DevTools** — Cmd+Opt+Iで開き、Elementsタブで`ld+json`を検索。


WARNING


構造化データに**実際のページコンテンツと矛盾する情報**を記述すると、Googleからスパム判定を受ける可能性があります。例えば、ページ上に存在しないレビュー数や、実際と異なる価格をマークアップしないでください。


## AI引用率を最大化する5つのベストプラクティス


*AI引用率最大化のための5つのベストプラクティスを示す上昇グラフ*


1. ネスト構造を活用する
単純なフラットなJSON-LDではなく、Organizationの中にContactPointやPostalAddressをネストすることで、情報の関連性をAIが正確に理解できる。

2. sameAsプロパティで外部リンクを明示する
Twitter、Facebook、WikipediaなどのURLをsameAsで指定することで、エンティティの信頼性が向上する。LLMは複数の情報源からの裏取りを行うため、この指定が引用確率に影響する。104社調査ではWikipedia掲載企業の引用率が2.7倍だった。

3. @idで一意識別子を付与する
同じサイト内の複数ページで同一エンティティを参照する場合、"@id": "https://example.com/#organization"のように共通IDを使うことで、AIがサイト全体の情報を統合しやすくなる。

4. @graphで複数スキーマを1ブロックにまとめる
1ページに複数のJSON-LDスクリプトタグを散在させるより、@graph配列で1つのブロックにまとめたほうがAIのパース効率が高い。

5. dateModifiedを定期更新する
Article や Product のdateModifiedを実際の更新日に合わせて更新する。AIは情報の鮮度を重視するため、古いdateModifiedのままでは引用優先度が下がる。


## よくある実装ミスと対策


| ミスの内容 | 影響 | 対策 |
| --- | --- | --- |
| 必須プロパティの欠落 | リッチリザルト非表示 | Schema.org公式ドキュメントで必須項目を確認 |
| datePublishedが未設定 | 記事の鮮度判定不能 | ISO 8601形式（YYYY-MM-DD）で必ず設定 |
| 画像URLが相対パス | クロール時に解決不能 | 絶対URL（https://始まり）を使用 |
| 複数タイプの競合 | 解析エラー | @graph配列で正しく格納 |
| ページ内容との不一致 | スパム判定リスク | マークアップ内容がページ上に実在することを確認 |
| priceValidUntilが過去の日付 | リッチリザルト非表示 | 必ず未来の日付を設定し、四半期ごとに更新 |


## 構造化データ実装チェックリスト


自社サイトの現状を以下のチェックリストで確認してほしい。104社調査では、これらを5つ以上クリアしている企業は全体の12%に過ぎなかった。


:::message
**構造化データ GEO対策チェックリスト**
- `Organization`スキーマに`sameAs`（SNS + Wikipedia）が設定されているか
- 商品ページに`Product` + `Offer` + `AggregateRating`があるか
- FAQページに`FAQPage`スキーマ（3問以上）があるか
- 記事ページに`Article` + `dateModified`が設定されているか
- OGPタグ（`og:title`、`og:description`、`og:image`）が全主要ページにあるか
- Google Rich Results Testでエラーが0件か
- `@id`でサイト内のエンティティが一意に識別されているか
:::


### 自社サイトのAI引用率を確認する


構造化データの実装状況を含め、ChatGPT・Claude・Gemini・Perplexityでの引用率を即座にスコア化。104社のベンチマークデータと比較できます。

無料で診断する


:::message
**関連記事**
- [AI検索対策の完全ガイド — GEO/LLMO実践](https://ai-visibility-index.dev/blog/what-is-geo/)
- [GEO対策の始め方 — 初日から180日のロードマップ](https://ai-visibility-index.dev/blog/geo-getting-started/)
- [AI Overviewに自社が出ない？ — Gemini対策ガイド](https://ai-visibility-index.dev/blog/ai-overview-guide/)
- [ChatGPTに自社名が出ないを解決する7つの方法](https://ai-visibility-index.dev/blog/chatgpt-citation-methods/)
- [Perplexityに企業名が表示されない？対策5選](https://ai-visibility-index.dev/blog/perplexity-visibility-guide/)
- [AI検索で見えない企業の5つの共通点](https://ai-visibility-index.dev/blog/invisible-companies/)
:::

---

> この記事は [AI Visibility Index](https://ai-visibility-index.dev) の調査データに基づいています。EC業界104社のAI可視性ランキングは[こちら](https://ai-visibility-index.dev)で公開中です。
