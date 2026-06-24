---
title: "Perplexityに企業名が表示されない？ — 104社データで判明した対策5選"
emoji: "🔍"
type: "tech"
topics: ["Perplexity","AI","SEO","マーケティング","データ分析"]
published: true
---

*Perplexityでの企業可視性を示すイラスト — 検索結果とAI回答の接続*


「Perplexityで自社名を検索したら、まったく出てこなかった」。そんな体験をした企業は少なくないはずだ。


Perplexityは、ChatGPTやClaudeとは根本的に異なるAI検索エンジンだ。リアルタイムのWeb検索結果をAIが要約して回答するハイブリッド型のアーキテクチャを採用しており、従来のGEO対策だけでは不十分なケースが多い。


本記事では、**EC業界104社の独自調査データ**からPerplexityの引用傾向を分析し、このエンジンに特化した5つの対策アクションを解説する。


## Perplexityとは何か — 他のAIエンジンとの決定的な違い


Perplexity AI（パープレキシティ）は「AI搭載の検索エンジン」を標榜するサービスだ。ChatGPTやClaudeが学習データに基づいて回答を生成するのに対し、**Perplexityはリアルタイムのウェブ検索結果をソースとして回答を組み立てる**。


:::message
**Perplexityのアーキテクチャ**
- **リアルタイムWeb検索:** ユーザーの質問に対して、まずWeb検索を実行し、最新の情報を収集する
- **RAG（検索拡張生成）:** 検索結果をLLMに渡し、情報を統合・要約して回答を生成する
- **ソース明示:** 回答の根拠となったWebページのURLを必ず表示する（他のAIにはない特徴）
:::


この設計の結果、Perplexityは他のAIエンジンと比べて以下の特性を持つ:


- **最新情報に強い:** 学習データの時点に依存しないため、昨日公開されたプレスリリースでも引用される可能性がある
- **検索順位の影響を受ける:** Web検索結果の上位ページを優先的に参照するため、SEOの成果が直接反映されやすい
- **OGPタグへの感度が高い:** 検索結果のプレビュー（title/description/image）を回答の組み立てに活用する


## 104社調査で見えたPerplexityの引用実態


[AI Visibility Index](https://ai-visibility-index.dev/)の2026年5月度調査（104社）から、Perplexityの引用パターンを分析する。


:::message
**Perplexity引用データ（104社）**
- **全体平均:** 1.1点 / 100点（4エンジン中3位）
- **完全に0点の企業:** 17社（16%）— 一度もPerplexityに引用されない
- **1点未満:** 81社（78%）— ほぼ「見えない」状態
- **5点以上:** わずか3社（3%）— Amazon(15.6), 楽天(14.9), Yahoo!(8.3)
:::


### Perplexity Top 5 企業


| 順位 | 企業 | Perplexityスコア | ChatGPTスコア | 差分 |
| --- | --- | --- | --- | --- |
| 1 | **Amazon Japan** | 15.6 | 42.0 | -26.4 |
| 2 | **楽天市場** | 14.9 | 35.5 | -20.6 |
| 3 | **Yahoo!ショッピング** | 8.3 | 13.5 | -5.2 |
| 4 | **ヨドバシ.com** | 4.8 | 4.1 | +0.7 |
| 5 | **STORES** | 4.1 | 2.9 | +1.2 |


注目すべきポイントが2つある。


第一に、Perplexityのトップ3はChatGPTと同じ（Amazon、楽天、Yahoo!）だが、スコア差が大きく縮まる。ChatGPTが大手モール型を圧倒的に優遇するのに対し、Perplexityは比較的フラットに引用する傾向がある。


第二に、**ヨドバシ.com（+0.7）とSTORES（+1.2）はPerplexityのスコアがChatGPTを上回っている**。この「逆転現象」がPerplexity対策の鍵になる。


## Perplexityで逆転が起きる理由 — 47社の分析


104社中、**PerplexityスコアがChatGPTスコアを上回る企業は47社**に達する。これは全体の45%にあたり、ほぼ半数の企業がPerplexityのほうが「見えやすい」状態にあることを意味する。


逆転企業に共通するパターンを分析すると、以下の特徴が浮かび上がる。


:::message
**Perplexity逆転企業の共通特徴**
- **テック系メディアでの露出が多い:** STORES、BASE、Shopify Japanなどのプラットフォーム企業は、テック系メディアの記事に頻繁に登場する
- **ニッチ領域での専門性が高い:** FABRIC TOKYO（1.2 vs 0.0）やLOWYA（2.5 vs 0.2）など、特定領域のスペシャリストが強い
- **プレスリリースの発信頻度が高い:** Perplexityのリアルタイムクローリングに拾われやすい
- **OGPタグの実装が完全:** 検索結果のプレビュー品質がPerplexityの参照確率に影響する
:::


つまり、ChatGPTでは大手に勝てない中小企業やD2Cブランドでも、**Perplexityでは適切な戦略で可視性を獲得できる可能性がある**。


### 自社のPerplexityスコアを確認する


ChatGPT・Claude・Gemini・Perplexityの4エンジンで、あなたの企業がどの程度「見えている」かを即座にスコア化。

無料で診断する


## Perplexity可視性を高める5つのアクション


### アクション1: プレスリリースの最適化


Perplexityはリアルタイムのウェブ検索を基盤としているため、**最新のプレスリリースが引用される確率が他のAIエンジンより圧倒的に高い**。PR TIMESやPR WIRE等のプレスリリース配信サービスを月1回以上の頻度で活用する。


ポイントは、プレスリリースのタイトルと冒頭文に**検索されやすいキーワード**を含めること。「新商品発売のお知らせ」ではなく、「〇〇業界初の△△機能搭載 — □□の課題を解決」のように、ユーザーが質問しそうなフレーズを含めるとPerplexityに拾われやすくなる。


### アクション2: OGPタグの完全実装


Perplexityは104社調査でOGPタグ実装企業を**3.6倍**引用する傾向が確認されている。これは4エンジン中で最も高い倍率だ。


最低限、以下のOGPタグを全主要ページに設定する。


- `og:title` — 検索意図に合致する明確なタイトル
- `og:description` — ページの価値を端的に伝える120文字以内の説明
- `og:image` — 1200x630pxの高品質な画像
- `og:type` — article / product / website の適切な指定
- `og:site_name` — ブランド名の統一表記


### アクション3: テック系メディアでの露出


逆転企業の分析から、テック系メディア（TechCrunch Japan、BRIDGE、日経クロステック、ITmedia等）での記事掲載がPerplexityスコアに強く影響することが示唆されている。


自社のプロダクトやサービスに関連する記事ネタ（業界トレンド分析、調査レポート、専門家コメント）をメディアに提供し、被引用の機会を作る。寄稿の提案書を準備し、定期的にアプローチする。


### アクション4: FAQ構造化データの実装


PerplexityはFAQPage構造化データを認識し、質問と回答のペアを回答生成の材料として利用する。特に「〇〇とは」「〇〇の違い」といった定義型の質問に対して、構造化データ付きのFAQは引用されやすい。


自社サイトの主要ページ（トップ、商品カテゴリ、会社概要）にFAQPage JSON-LDを追加する。質問は実際にユーザーが検索エンジンに投げるフレーズを使うこと。


構造化データの詳しい実装方法は[構造化データ完全ガイド](https://ai-visibility-index.dev/blog/structured-data-guide/)を参照。


### アクション5: llms.txt対応


`llms.txt`はサイトルートに配置するテキストファイルで、AIクローラーに対してサイトの概要・主要ページ・専門領域を伝える新しい標準だ。Perplexityのクローラーもこのファイルを参照する可能性がある。


設置は簡単で、サイトの`robots.txt`と同じディレクトリに以下の形式で配置する。

# llms.txt
# [サイト名]

## サイト概要
[サイトの概要と専門領域]

## 主要ページ
- [ページ名]: [URL] - [概要]

## 専門領域
[自社が持つ専門知識・データ]


## Perplexity対策の優先順位 — スコア帯別ロードマップ


| 現在のスコア | 最優先アクション | 期待される効果 | 所要期間 |
| --- | --- | --- | --- |
| **0点**（17社） | OGPタグ実装 + プレスリリース1本 | 0→0.1〜0.5点 | 1〜2ヶ月 |
| **0.1〜1点**（64社） | FAQ構造化データ + llms.txt + 月1回PR | 1〜2点台への上昇 | 2〜3ヶ月 |
| **1〜5点**（20社） | メディア露出強化 + 専門コンテンツ量産 | 5点超え（Top 10入り可能） | 3〜6ヶ月 |
| **5点超**（3社） | エンジン横断の総合GEO戦略 | 業界リーダーポジション確立 | 継続的 |


## まとめ：Perplexityは中小企業の突破口になる


Perplexityはリアルタイム検索ベースのため、**ChatGPTやClaudeでは大手企業に太刀打ちできない中小企業やD2Cブランドにとって、最も可能性のあるAI検索チャネル**だ。


104社調査から見えたポイントをまとめる。


- Perplexity平均スコアは1.1点。**104社中78%がほぼ見えない**（1点未満）
- 45%の企業でPerplexityスコアがChatGPTを上回る — **逆転の可能性**がある
- OGPタグ実装企業はPerplexityで**3.6倍引用される**（4エンジン中最高）
- プレスリリースとテック系メディア露出が最も効くチャネル
- **0点からでも、OGP + PR 1本で引用ゼロを脱出できる**


まずは[無料チェッカー](https://ai-visibility-index.dev/tools/)で自社のPerplexityスコアを確認し、上記の5つのアクションから着手してほしい。GEO全体の対策は[AI検索対策の完全ガイド](https://ai-visibility-index.dev/blog/what-is-geo/)を参照。


:::message
**関連記事**
- [AI検索対策の完全ガイド — GEO/LLMO実践](https://ai-visibility-index.dev/blog/what-is-geo/)
- [ChatGPTに自社名が出ない を解決する7つの方法](https://ai-visibility-index.dev/blog/chatgpt-citation-methods/)
- [ChatGPTに自社が出てこない企業の5つの共通点](https://ai-visibility-index.dev/blog/invisible-companies/)
- [構造化データ完全ガイド — Schema.orgでAI可視性を高める](https://ai-visibility-index.dev/blog/structured-data-guide/)
- [ChatGPT vs Perplexity vs Gemini — AIエンジン別の引用傾向](https://ai-visibility-index.dev/blog/engine-industry-cross-analysis/)
- [GEO対策の始め方 — 初日から180日のロードマップ](https://ai-visibility-index.dev/blog/geo-getting-started/)
:::

---

> この記事は [AI Visibility Index](https://ai-visibility-index.dev) の調査データに基づいています。EC業界104社のAI可視性ランキングは[こちら](https://ai-visibility-index.dev)で公開中です。
