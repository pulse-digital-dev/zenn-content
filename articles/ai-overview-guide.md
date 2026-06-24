---
title: "AI Overviewに自社が出ない？ — Gemini平均0.6点の衝撃データと今すぐ始める5つの対策"
emoji: "🤖"
type: "tech"
topics: ["Gemini","AI","SEO","GoogleAI","マーケティング"]
published: true
---

*Google AI Overviewの検索結果にAI生成回答が表示されるイメージ*


Google検索の「AI Overview」。EC業界104社を調査した結果、このGoogleのAI機能で引用される企業はわずか12社。Geminiの平均スコアは0.6点で、4大AIエンジン中最も低い。92社が1点にすら届いていない。


AI Overviewは、Google検索結果の最上部に表示されるAI生成回答だ。これまでの検索結果1位よりもさらに上に表示されるため、ここに引用されるかどうかが今後のオーガニック流入を左右する。しかし現時点で、日本のEC企業のAI Overview対策はほぼ未着手の状態にある。


NOTE


この記事はGoogleのAI Overview（AIO）に特化した対策ガイドです。AI検索対策全般は[GEO/LLMO/AIO完全ガイド](https://ai-visibility-index.dev/blog/what-is-geo/)を、他のエンジン別対策は[Perplexity対策](https://ai-visibility-index.dev/blog/perplexity-visibility-guide/)・[ChatGPT対策](https://ai-visibility-index.dev/blog/chatgpt-citation-methods/)をご覧ください。


## AI Overviewとは — Google検索のAI化


AI Overview（旧SGE: Search Generative Experience）は、2024年にGoogleが導入した検索機能だ。ユーザーの検索クエリに対して、AIが複数のWebページから情報を統合・要約し、検索結果の最上部に表示する。


:::message
**AI Overviewの特徴**
- **表示位置:** 検索結果の最上部（広告の直下、オーガニック結果の上）
- **情報源:** Google検索でインデックスされた上位ページをAI（Gemini）が統合
- **ソース表示:** 引用元のWebページをカード形式で表示
- **影響:** AI Overviewだけで回答が完結するため「ゼロクリック検索」が加速
:::


従来のSEOでは「Google検索1位」が最大の目標だった。しかしAI Overviewの導入により、「検索1位のさらに上」に表示される新たなポジションが生まれた。ここに引用されるかどうかが、次の競争の軸になる。


## 104社データで見るGeminiの特異性


GoogleのAI OverviewはGeminiモデルをベースにしている。[AI Visibility Index](https://ai-visibility-index.dev/)ではGeminiを直接テストしており、このデータがAI Overview対策の基盤となる。


### Gemini vs 他エンジン — 平均スコア比較


| AIエンジン | 平均スコア | 1点以上の企業数 | 0点の企業数 | 特徴 |
| --- | --- | --- | --- | --- |
| **ChatGPT** | 1.8点 | 24社 | 30社 | ブランド認知を最も反映 |
| **Claude** | 1.4点 | 18社 | 38社 | 専門性・コンテンツ品質を重視 |
| **Perplexity** | 1.1点 | 20社 | 17社 | リアルタイムWeb検索ベース |
| **Gemini** | **0.6点** | **12社** | — | 最も保守的。EC企業をほぼ引用しない |


Geminiの平均スコア0.6点は、ChatGPT（1.8点）の**3分の1**に過ぎない。104社中、Geminiで1点以上を獲得している企業はわずか12社。残りの92社は実質的に「Geminiから見えない」状態だ。


### Gemini 1点以上の12社 — 共通パターン分析


| 企業 | Geminiスコア | 総合スコア | 業界 |
| --- | --- | --- | --- |
| **Amazon Japan** | 13.2 | 28.1 | 総合EC |
| **楽天市場** | 7.8 | 23.1 | 総合EC |
| Yahoo!ショッピング | 3.9 | 10.5 | 総合EC |
| THREE | 2.5 | 0.7 | コスメ |
| Minimal | 2.1 | 1.5 | D2C |
| ZOZOTOWN | 2.0 | 6.2 | ファッション |
| STORES | 1.9 | 2.7 | 総合EC |
| ヨドバシ.com | 1.9 | 3.9 | 家電 |
| オイシックス | 1.4 | 4.0 | 食品 |
| IKEA Japan | 1.3 | 2.7 | インテリア |
| ニトリ | 1.3 | 3.8 | インテリア |
| メルカリ | 1.0 | 5.6 | 総合EC |


この12社から読み取れるパターンがある。


- **大手モール型が優位:** 12社中5社が総合EC（Amazon、楽天、Yahoo!、STORES、メルカリ）
- **THREE（Gemini 2.5 vs 総合0.7）の逆転:** コスメ業界でGeminiスコアだけが突出。構造化データの実装品質が高い可能性
- **Minimal（Gemini 2.1 vs 総合1.5）:** D2C唯一のGemini 1点超え。ニッチ領域での専門性がGeminiに評価された可能性
- **各業界から1社ずつ:** Geminiは「業界代表」を1社だけ引用する傾向がある


### 自社のGeminiスコアを確認する


ChatGPT・Claude・Gemini・Perplexityの4エンジンで、あなたのサイトがどの程度「見えている」かを即座にスコア化。AI Overview対策の出発点に。

無料で診断する


## AI Overviewに引用されるための5つの施策


104社のデータとGeminiの引用傾向から、AI Overview対策として有効な施策を優先度順に解説する。


### 施策1: 構造化データ（JSON-LD）の徹底実装


Geminiは4エンジン中、**構造化データに最も強く反応する**。JSON-LD実装企業のGemini引用率は未実装企業の2.0倍だ（他エンジンは1.2〜1.5倍）。


特に効果が高いスキーマタイプは:


- **FAQPage:** Geminiが直接回答の材料として参照する確率が高い
- **HowTo:** 手順型のクエリに対してGeminiが優先的に引用
- **Product + Offer:** 商品比較クエリで引用されるための基盤
- **Organization:** エンティティの信頼性の土台


実装の詳細は[構造化データ完全ガイド](https://ai-visibility-index.dev/blog/structured-data-guide/)を参照。EC業種別のJSON-LDテンプレートもコピペで使える。


### 施策2: Google検索1ページ目のコンテンツ最適化


AI Overviewは**Google検索結果の上位ページから情報を取得する**。つまり、従来のSEOの成果がAI Overview対策の基盤になる。


具体的なアクションとしては:


- ターゲットキーワードでGoogle検索1ページ目に入っていない場合、まずSEO対策を先行させる
- 既に上位表示されているページのコンテンツを、AIが引用しやすい形式に最適化する（FAQの追加、段落構造の明確化）
- E-E-A-T（経験・専門性・権威性・信頼性）のシグナルを強化する


### 施策3: E-E-A-Tシグナルの強化


Geminiは引用元の信頼性を厳しく評価する。104社中92社がGemini 1点未満という極端な選別性は、Geminiの「信頼できるソース以外は引用しない」というポリシーの表れだ。


E-E-A-T強化の具体策:


- **著者プロフィール:** 記事に著者名・専門分野・経歴を明示する。JSON-LDの`author`プロパティに反映
- **編集ポリシー:** 情報の正確性を担保する編集方針を公開する
- **引用と出典:** 記事内で統計やデータを引用する際、出典を明示する
- **運営者情報:** 会社概要ページに代表者名・住所・連絡先を掲載する


### 施策4: FAQコンテンツの体系的な構築


AI Overviewは「質問に対する直接的な回答」を提示する機能だ。そのため、**ユーザーの質問に1対1で回答するFAQコンテンツ**が最も引用されやすい。


効果的なFAQの作り方:


- 1カテゴリあたり10問以上のFAQを用意する
- 質問は実際の検索クエリに近い自然な文体にする（例: 「ノイキャンイヤホンの選び方は？」）
- 回答は150〜300文字で完結させる。「詳しくはこちら」で終わらせない
- FAQPage JSON-LDを必ず付与する
- 四半期ごとに質問と回答を見直し、`dateModified`を更新する


### 施策5: サイトマップとクロール許可の明示


AI OverviewのデータソースはGoogle検索インデックスだ。Googlebot のクロールを適切に許可し、サイト構造を正しく伝えることが前提条件となる。


- `sitemap.xml`を最新の状態に保ち、Google Search Consoleに送信する
- `robots.txt`で重要ページのクロールをブロックしていないことを確認する
- `Google-Extended`（AIトレーニング用クローラー）のブロックがAI Overviewの表示にも影響する可能性に注意
- `llms.txt`をサイトルートに配置し、AIクローラーにサイトの概要を伝える


## AI Overview vs ChatGPT vs Perplexity — 対策の違い


3つの主要AI検索チャネルは、それぞれ情報ソースとアルゴリズムが異なる。対策も一律ではなく、チャネルごとの最適化が必要だ。


| 項目 | AI Overview (Gemini) | ChatGPT | Perplexity |
| --- | --- | --- | --- |
| **情報ソース** | Google検索インデックス | 学習データ + Web検索 | リアルタイムWeb検索 |
| **引用の厳しさ** | 最も厳格（12/104社のみ） | 中程度（24/104社） | 最も寛容（0点が17社のみ） |
| **最重要対策** | 構造化データ + Google SEO | Wikipedia + ブランドメンション | OGPタグ + プレスリリース |
| **効果が出るまで** | 3〜6ヶ月 | 6ヶ月〜 | 1〜2ヶ月 |
| **中小企業の可能性** | 構造化データで一定の勝機 | 大手有利、参入障壁高い | 最もチャンスが大きい |


TIP


すべてのエンジンを同時に対策する必要はありません。まずPerplexity（効果が早い）から始め、構造化データでGemini/AI Overview対策を固め、最後にChatGPT向けのブランド施策に取り組むのが効率的な順序です。


## まとめ: AI Overviewは「次のSEO」の主戦場


104社の調査データが示す現実は厳しい。Gemini平均0.6点、1点以上はわずか12社。しかし裏を返せば、**ほぼ全員がスタートラインに立っていない**ということでもある。


- **Gemini平均0.6点** — 4エンジン中最も低く、対策が最も遅れている領域
- **Gemini 1pt以上は12社のみ** — 競合が少ない = 先行者利益のチャンス
- **構造化データが最も効くエンジン** — JSON-LD実装で引用率2.0倍
- **THREE（2.5pt）やMinimal（2.1pt）** — ニッチ領域でも突破口はある
- Google SEOの延長線上にある対策のため、**既存のSEO資産を活用**できる


まずは[無料チェッカー](https://ai-visibility-index.dev/tools/)で自社のGeminiスコアを確認し、[構造化データガイド](https://ai-visibility-index.dev/blog/structured-data-guide/)のテンプレートから実装を始めてほしい。GEO対策の全体ロードマップは[GEO対策の始め方](https://ai-visibility-index.dev/blog/geo-getting-started/)で解説している。


:::message
**関連記事**
- [AI検索対策の完全ガイド — GEO/LLMO実践](https://ai-visibility-index.dev/blog/what-is-geo/)
- [GEO対策の始め方 — 初日から180日のロードマップ](https://ai-visibility-index.dev/blog/geo-getting-started/)
- [構造化データでAI検索引用率を上げる完全ガイド](https://ai-visibility-index.dev/blog/structured-data-guide/)
- [Perplexityに企業名が表示されない？対策5選](https://ai-visibility-index.dev/blog/perplexity-visibility-guide/)
- [ChatGPTに自社名が出ないを解決する7つの方法](https://ai-visibility-index.dev/blog/chatgpt-citation-methods/)
- [ECサイトのAI検索対策2026 — 業界別格差の最新分析](https://ai-visibility-index.dev/blog/geo-llmo-ec-market-2025/)
:::

---

> この記事は [AI Visibility Index](https://ai-visibility-index.dev) の調査データに基づいています。EC業界104社のAI可視性ランキングは[こちら](https://ai-visibility-index.dev)で公開中です。
