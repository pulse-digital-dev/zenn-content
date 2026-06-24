---
title: "llms.txtの書き方ガイド — EC業界104社中ほぼゼロの今が先行者優位のチャンス"
emoji: "📄"
type: "tech"
topics: ["AI","LLM","SEO","webdev","マーケティング"]
published: true
---

llms.txtは、AIエージェントやLLMがWebサイトの情報を効率的に取得するための新しい標準仕様だ。robots.txtがクローラー向けのルールファイルであるように、llms.txtはAI向けの「サイト案内書」として機能する。AI Visibility Indexの[GEO/LLMO](https://ai-visibility-index.dev/blog/what-is-geo/)調査で104社のECサイトを分析した結果、llms.txtを設置している企業はほぼ皆無であることが判明した。つまり、今設置すれば先行者優位を確保できる。


## llms.txtとは何か


llms.txtは、2024年にJeremy Howard氏（fast.ai創設者）が提唱した仕様で、LLM（大規模言語モデル）がWebサイトのコンテンツを理解しやすい形で提供するためのMarkdownファイルだ。


従来のWebページは人間向けにデザインされており、ナビゲーション、広告、JavaScript、CSSなど大量のノイズを含む。LLMがこれらを正確に解析するのは非効率であり、結果としてサイトの情報が正しくAIに伝わらないリスクがある。llms.txtは、この問題を解決するために「AI向けの構造化された要約」をサイトルートに設置する仕組みだ。


:::message
**llms.txt vs robots.txt vs sitemap.xml**
- **robots.txt** — クローラーに「どこを見てよいか」を指示するルールファイル
- **sitemap.xml** — クローラーに「どのURLが存在するか」を伝えるインデックス
- **llms.txt** — LLMに「このサイトの本質的な情報」をMarkdown形式で伝えるAI向け案内書
:::


## llms.txtの基本フォーマット


llms.txtはサイトルート（`https://example.com/llms.txt`）に設置するMarkdownファイルで、以下の構造を持つ。


```
# サイト名

> サイトの1行要約（LLMが最初に読むコンテキスト）

## セクション名

- [ページタイトル](URL): ページの説明
- [ページタイトル](URL): ページの説明

## Optional

- [補足ページ](URL): 必要に応じて参照
```


:::message
**llms.txt仕様のポイント**
- フォーマットはMarkdown（.md記法）
- 最初の `# 見出し` がサイト名
- `>` でサイトの要約を記述（blockquote）
- 各セクションでURLとその説明をリスト形式で列挙
- `## Optional` セクションで補足情報を提供
- Content-Typeは `text/plain` または `text/markdown`
:::


## 104社のllms.txt実装状況 — ほぼゼロの現実


AI Visibility Indexの5月度調査（104社）で各社のllms.txt設置状況を確認したところ、設置が確認できた企業はほぼ存在しなかった。これは日本のEC業界において、llms.txtという概念自体がまだ認知されていないことを意味する。


**llms.txtの現状 — 国内EC業界の技術対応状況**

| 技術シグナル | 実装率 | AI引用との相関 |
| --- | --- | --- |
| Wikipedia掲載 | **70%** | 2.7倍 |
| OGPタグ | 44% | 2.7倍 |
| ブログ/メディア | 41% | 2.3倍 |
| JSON-LD | 23% | 1.4倍（Geminiで2.0倍） |
| llms.txt | **1%未満** | 未計測（設置数不足） |


逆に言えば、今llms.txtを設置すれば、**業界の99%以上が対応していない施策を先取りできる**。AIエンジンがllms.txtを本格的に参照し始めた時点で、先行企業は大きなアドバンテージを得ることになる。


## ECサイト向けllms.txtテンプレート


EC業界の特性に最適化したllms.txtテンプレートを5業種分用意した。自社の業種に合わせてカスタマイズしてほしい。


### ファッションEC向け


```
# ブランド名 公式オンラインストア

> 日本発のファッションブランド。レディース・メンズのアパレル、
> シューズ、バッグを企画・製造・販売。全国XX店舗とオンラインストアを展開。

## 主要コンテンツ

- [新作コレクション](https://ai-visibility-index.dev/new-arrivals/): 最新シーズンの商品一覧
- [レディース](https://ai-visibility-index.dev/women/): レディースカテゴリトップ
- [メンズ](https://ai-visibility-index.dev/men/): メンズカテゴリトップ
- [セール](https://ai-visibility-index.dev/sale/): セール・アウトレット商品
- [店舗一覧](https://ai-visibility-index.dev/stores/): 全国店舗の所在地と営業時間

## ブランド情報

- [ブランドストーリー](https://ai-visibility-index.dev/about/): 創業理念と沿革
- [サステナビリティ](https://ai-visibility-index.dev/sustainability/): 環境への取り組み
- [よくある質問](https://ai-visibility-index.dev/faq/): サイズガイド、配送、返品について

## Optional

- [プレスリリース](https://ai-visibility-index.dev/press/): メディア向け情報
- [採用情報](https://ai-visibility-index.dev/careers/): 求人情報
```


### 食品EC向け


```
# サービス名

> 食材宅配サービス。産地直送の有機野菜・ミールキットを
> 定期便で届ける。会員数XX万人。

## 主要コンテンツ

- [商品カテゴリ](https://ai-visibility-index.dev/products/): 野菜セット、ミールキット、加工品
- [定期便の仕組み](https://ai-visibility-index.dev/subscription/): プランと配送スケジュール
- [産地情報](https://ai-visibility-index.dev/farmers/): 契約農家と生産者紹介
- [レシピ](https://ai-visibility-index.dev/recipes/): ミールキットのレシピ集

## 企業情報

- [食の安全への取り組み](https://ai-visibility-index.dev/safety/): 品質管理基準
- [よくある質問](https://ai-visibility-index.dev/faq/): 注文方法、配送エリア、アレルギー対応

## Optional

- [お客様の声](https://ai-visibility-index.dev/reviews/): 利用者レビュー
```


### 汎用EC向け（最小構成）


```
# ストア名

> 事業内容を1-2行で。取扱商品、ターゲット顧客、独自の強み。

## 主要ページ

- [商品一覧](https://ai-visibility-index.dev/products/): 全カテゴリの商品
- [会社概要](https://ai-visibility-index.dev/about/): 事業概要と沿革
- [FAQ](https://ai-visibility-index.dev/faq/): よくある質問

## Optional

- [ブログ](https://ai-visibility-index.dev/blog/): お知らせ・コラム
```


## llms.txt + JSON-LD + OGP — AI可視性の三位一体戦略


[技術要因分析](https://ai-visibility-index.dev/blog/technical-factors-ai-visibility/)で明らかになったように、AIに引用されるためには複数の技術シグナルの「組み合わせ」が重要だ。llms.txtは、既存の[構造化データ（JSON-LD）](https://ai-visibility-index.dev/blog/structured-data-guide/)やOGPタグと組み合わせることで、最大の効果を発揮する。


**AI可視性の三位一体 — 各技術の役割**

| 技術 | 対象 | 伝える情報 | AIエンジンへの影響 |
| --- | --- | --- | --- |
| **JSON-LD** | Google / Gemini | 構造化されたエンティティ情報 | Geminiで引用率2.0倍 |
| **OGP** | Perplexity / SNS | ページの要約とメタ情報 | Perplexityで引用率3.6倍 |
| **llms.txt** | ChatGPT / Claude / エージェント | サイト全体の構造と概要 | 先行者優位の確保 |


この三位一体を実装することで、4大AIエンジンすべてに対して最適化されたシグナルを送ることができる。特にllms.txtは、今後AIエージェントが自律的にWebを巡回する時代において、サイトの「第一印象」を決定する重要な要素になると考えられる。


## llms.txt設置時の注意点とベストプラクティス


:::message
**5つのベストプラクティス**
- **簡潔に書く** — LLMのコンテキストウィンドウには限りがある。重要な情報を優先し、全ページを列挙しない
- **正確な情報を記載する** — 古い情報や誇大表現はAIの信頼性評価を下げる。事実に基づいた記述のみ
- **定期的に更新する** — 商品ラインナップやサービス内容が変わったら即座に反映する
- **robots.txtでブロックしない** — llms.txtがAIクローラーにアクセスできるよう、robots.txtでの除外に注意する
- **llms-full.txtも検討する** — llms.txtが要約なのに対し、llms-full.txtはサイト全体の詳細情報を提供するオプション。大規模サイト向け
:::


よくある実装ミス


- **HTMLで書いてしまう** — llms.txtはMarkdown形式。HTMLタグを使うとAIが正しくパースできない
- **全ページをリストする** — 数千の商品ページを全て列挙するのは逆効果。カテゴリトップやFAQなど、AIが参照すべき代表ページに絞る
- **機密情報を含める** — 管理画面URL、内部APIエンドポイントなどを記載しない
- **設置後に放置する** — セール終了後もセールページを「主要コンテンツ」に残すなど、情報の鮮度が落ちると逆効果


## まとめ — 今日から始めるllms.txt戦略


llms.txtはまだ黎明期の技術だが、AIエージェントの急速な普及を考えれば、早期対応のメリットは大きい。


- **設置コストはほぼゼロ** — 1つのMarkdownファイルをサイトルートに配置するだけ
- **競合はほぼ未対応** — 104社調査でllms.txt設置率は1%未満
- **既存施策と相乗効果** — [JSON-LD](https://ai-visibility-index.dev/blog/structured-data-guide/) + OGP + llms.txtの三位一体で全AIエンジンをカバー


まずは上記テンプレートをベースに、自社のllms.txtを作成してみてほしい。設置後は[無料チェッカー](https://ai-visibility-index.dev/tools/)で現在のAI可視性スコアを計測し、改善の基準値を把握しておこう。


### 自社のAI可視性を無料でチェック


llms.txt設置前後のスコア変化を追跡するためにも、まず現状を把握しましょう。4大AIエンジンでの引用率を無料で診断できます。

無料でスコアを確認 &#8594;


関連記事


- [AI検索対策の完全ガイド（GEO/LLMO/AIO）](https://ai-visibility-index.dev/blog/what-is-geo/)
- [構造化データでAI検索引用率を上げる完全ガイド](https://ai-visibility-index.dev/blog/structured-data-guide/)
- [GEO対策の始め方 — 初日から180日のロードマップ](https://ai-visibility-index.dev/blog/geo-getting-started/)
- [AIに引用される企業の技術的法則](https://ai-visibility-index.dev/blog/technical-factors-ai-visibility/)

---

> この記事は [AI Visibility Index](https://ai-visibility-index.dev) の調査データに基づいています。EC業界104社のAI可視性ランキングは[こちら](https://ai-visibility-index.dev)で公開中です。
