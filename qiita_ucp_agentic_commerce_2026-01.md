---
title: "GoogleのUniversal Commerce Protocol(UCP)とは何か：発表内容の確認と、他社の“エージェントが買う”動き（2026年1月版）"
tags:
  - Google
  - Gemini
  - 検索
  - ECommerce
  - AIエージェント
private: false
updated_at: "2026-01-14"
---

## この記事でやること

SNSやニュースで流通している以下の要旨（英語）について、**一次情報（Google公式）＋第三者報道＋関連仕様**を突き合わせ、正確な形に直します。

> Google launched the Universal Commerce Protocol, an open standard that enables AI agents to complete shopping tasks across discovery, purchase, and support... (略)

結論から言うと、この要旨は**主要部分がGoogle公式の一次情報で確認できます**（ただし「どの範囲まで“今すぐ”できるのか」「対象が誰か」は、公式の“条件（eligible / waitlist / soon）”が重要なので、そこを丁寧に書き換えます）。

## 結論（超要約）

- Googleは「Universal Commerce Protocol（UCP）」を**“open standard”**として発表しています（Google公式ブログ：`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、開発者向け解説：`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。
- UCPは「発見→購入→購入後サポート」までを視野に入れ、**A2A / AP2 / MCP と互換**と説明されています（同上）。
- Googleは、UCPを使って **SearchのAI Mode と Geminiアプリ内で“eligible U.S. retailers”向けにチェックアウト機能を提供予定**、支払いは **Google Pay**、さらに **PayPalも“soon”**と述べています（Google公式ブログ：`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。
- 併せて **Business Agent**（検索上でブランドとチャット）と **Direct Offers**（AI Mode内での割引提示を可能にするGoogle Adsのパイロット）を説明しています（Google公式ブログ：同URL、Business Agentヘルプ：`https://support.google.com/brandprofile/answer/16410382`）。

## ファクトチェック（主張→一次情報）

### 1) UCPは「AIエージェントが買い物タスクを完遂するためのオープン標準」か？

GoogleはUCPを「a new open standard for agentic commerce」「open standard」「common language」などとして説明しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

また、開発者向け記事ではUCPを **open-source standard** とし、エージェント・事業者・決済事業者間の“共通言語/プリミティブ”を定義すると書いています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

### 2) 対象は discovery / purchase / support まで含むか？

Google公式ブログは「shopping journey — from discovery and buying to post-purchase support」と明示しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

UCP公式サイト側でも「discovering and buying to post purchase experiences」など、購入後の体験まで含む設計思想が示されています（`https://ucp.dev/`）。

### 3) UCPは Agent2Agent / Agent Payments Protocol と “works with” するか？

Google公式ブログは、UCPが既存プロトコル（Agent2Agent / Agent Payments Protocol / Model Context Protocol）と互換だと述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

開発者向け記事でも、AP2（`https://ap2-protocol.org/`）との互換、A2A（`https://a2a-protocol.org/latest/`）、MCP（`https://modelcontextprotocol.io`）やAPIでの統合方法に触れています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

補足として、A2A公式ドキュメントはA2Aを「open standard」としつつ、**Googleが開発してLinux Foundationに寄贈**した旨まで書いています（`https://a2a-protocol.org/latest/`）。

### 4) Shopify/Etsy/Wayfair/Target/Walmartと共同開発、Amex/Mastercard/Stripe/Visa等の支持は本当か？

Google公式ブログは、UCPが **Shopify, Etsy, Wayfair, Target, Walmart** と共同開発（co-developed）され、**American Express, Mastercard, Stripe, Visa**等を含む「more than 20」社にendorsedされたと述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

同様の記述は開発者向け記事にもあります（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

さらに、UCPは仕様・ドキュメントがGitHubで公開されている（＝少なくとも“オープンに読める仕様”として提供されている）ことも確認できます（`https://github.com/Universal-Commerce-Protocol/ucp`、`https://ucp.dev/`）。

### 5) SearchのAI Mode/Geminiアプリ内でチェックアウトを“直接”行うのか？支払いはGoogle Pay/PayPalか？

Google公式ブログは、UCPが「AI Mode in Search と Gemini app」上のチェックアウト機能を**“soon power”**し、対象が **eligible U.S. retailers** であること、支払いが **Google Pay**、さらに **PayPalもsoon** と述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

一方で、開発者向け記事では「Google has built the first reference implementation of UCP, to power a new buying experience...」と書かれており、現時点の“まずはGoogle実装”である点も重要です（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

また、Google for Developersのガイドは「GoogleのAI surfaces（Search, Gemini）上でtransactionを可能にする」こと、導入にはMerchant Centerや承認（waitlist/approval）が絡むことを示しています（`https://developers.google.com/merchant/ucp/guides`）。

### 6) Business Agent / Direct Offers は本当に発表されたか？

- **Business Agent**：Google公式ブログで「launching Business Agent」として説明され、さらにBrand profileヘルプでも「Business Agent is a conversational experience on Google Search...」と定義されています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://support.google.com/brandprofile/answer/16410382`）。
- **Direct Offers**：Google公式ブログが「Introducing Direct Offers」「This new Google Ads pilot allows advertisers... directly in AI Mode」として説明しています（同URL）。

## 背景：なぜ今「エージェントが買う」のが難しいのか（丁寧め解説）

“AIが商品を探してくれる”だけなら既に多くの体験があります。問題は**「買う」**の部分です。

Visaの解説記事は、検索・比較はできても「実決済に必要な資格情報、認証・認可、信頼の枠組み」が欠けている、と整理しています（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。

Google側はもう1つのボトルネックとして、事業者（小売）側が「すべてのAIプラットフォーム/エージェントごとに個別統合」することになりがちな **N×Nの統合地獄** を挙げています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

ざっくり言うと、エージェントコマースは次の3点が揃わないと社会実装が進みません。

- **相互運用性**：エージェント↔店舗/プラットフォームの共通言語（UCPの狙い）
- **決済の安全性/同意**：人の同意を取り、監査可能な形で支払いを実行（AP2等の狙い）
- **ツール接続の標準化**：エージェントが外部ツール/データへ安全に接続（MCPの狙い。Microsoft Copilot StudioもMCP接続を公式に案内：`https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp`）

## UCP・A2A・AP2・MCPの役割分担（誤解しやすいので明確化）

Google自身がUCPの説明の中で、既存標準との関係を明示しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

ここでは“何を解決するプロトコルか”の観点で整理します。

- **UCP（Universal Commerce Protocol）**：発見〜購入〜購入後までの“商取引の流れ”を、エージェントと事業者バックエンドの間で標準化（`https://ucp.dev/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）
- **A2A（Agent2Agent）**：エージェント同士が安全に通信・協調するための標準（`https://a2a-protocol.org/latest/`）
- **MCP（Model Context Protocol）**：エージェントがツール/データソースに接続するための標準。MCPの仕様・スキーマがGitHubで公開されている（`https://github.com/modelcontextprotocol/modelcontextprotocol`）
- **AP2（Agent Payments Protocol）**：エージェントが支払いを行う際の安全性・責任追跡・同意証跡（non-repudiable cryptographic audit trail 等）を設計する“open protocol”（`https://ap2-protocol.org/`）

## 他社の関連する動き（“Googleだけの話”ではない）

### 1) 決済ネットワーク側：Visaは「Visa Intelligent Commerce」を掲げる

Visaは「AI agents to buy securely and seamlessly」という文脈で「Visa Intelligent Commerce」を打ち出しています（`https://corporate.visa.com/en/products/intelligent-commerce.html`）。

より具体的な説明として、Visaのストーリー記事は「AI-ready credentials（トークン化等）」「passkey/指示・シグナル/不正検知」などを含む“信頼”の枠組みとして説明しています（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。

### 2) ウォレット/決済事業者：PayPalは“agentic commerce”を前面に出し、Mastercardと提携も

PayPalは「Mastercard and PayPal Join Forces To Accelerate Secure Global Agentic Commerce」として、Mastercard Agent PayとPayPal walletの統合に言及しています（`https://newsroom.paypal-corp.com/2025-10-27-Mastercard-and-PayPal-Join-Forces-To-Accelerate-Secure-Global-Agentic-Commerce`）。

また、同プレスリリースのメタ情報でも「agent-driven transactions」等の語が確認できます（同URL）。

### 3) 決済インフラ：Stripeは“agentic workflows”向けの公式ドキュメントを用意

Stripeは「Add Stripe to your agentic workflows」として、エージェントに決済/金融機能を組み込むためのドキュメントを公開しています（`https://docs.stripe.com/agents`）。

### 4) “エージェントにツールを与える”標準：MCPはMicrosoft Copilot Studioにも入ってきている

Microsoft Learnのドキュメントは、Copilot StudioでMCPサーバーのツール/リソースに接続してエージェントを拡張できる、と説明しています（`https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp`）。

これは、UCPのような“商取引の標準”とは別軸で、**エージェントが外部能力を安全に呼び出す標準が業界に広がっている**ことを示す材料になります。

## 第三者ソース（報道）での裏取り

Google公式だけでなく、以下の第三者メディアでもUCP/AI Mode/割引提示などが報じられています。

- TechCrunch: `https://techcrunch.com/2026/01/11/google-announces-a-new-protocol-to-facilitate-commerce-using-ai-agents/`
- Search Engine Land: `https://searchengineland.com/google-universal-commerce-protocol-467290`
- CNBC: `https://www.cnbc.com/2026/01/11/google-launches-universal-commerce-protocol-bets-on-ai-powered-retail.html`

（※報道は読める範囲が媒体の仕様に依存するため、**本稿の結論は一次情報を優先**しています。）

## まとめ：読み手が注意すべきポイント

- **“open standard”＝いますぐ誰でも同条件で使える、とは限らない**：GoogleのAI Mode/Gemini側のチェックアウトは「eligible」「soon」「承認が必要」など条件がある（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.google.com/merchant/ucp/guides`）。
- **“エージェントが買う”の難所は「同意・責任・不正対策・資格情報」**：Visaの説明が分かりやすい（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。
- **業界は分業で標準化が進んでいる**：UCP（商取引）、A2A（エージェント間）、MCP（ツール接続）、AP2（決済）という役割分担が見える（各公式URLは本文中）。

---

もし「技術者として次に何を見ればいいか」を深掘りするなら、まずは以下が入口になります。

- UCP仕様：`https://ucp.dev/specification/overview/`
- Google実装ガイド：`https://developers.google.com/merchant/ucp/guides`
- AP2概要：`https://ap2-protocol.org/`
- A2A概要：`https://a2a-protocol.org/latest/`

