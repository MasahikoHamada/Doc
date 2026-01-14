---
title: "AIが“買い物を代行する”時代の共通言語？ GoogleのUCPをやさしく読み解く（根拠リンク付き）"
tags:
  - Google
  - Gemini
  - 検索
  - ECommerce
  - AIエージェント
private: false
updated_at: "2026-01-14"
---

## まず最初に：この記事は「何を知ればOK？」の地図です

最近よく見る「AIが買い物を“代行”する」「検索の中でそのまま決済できる」系の話は、ワクワクする一方で、何が“確定”で何が“予定”なのかが混ざりがちです。

そこで本稿では、Googleが発表した **Universal Commerce Protocol（UCP）** を軸に、次を“読み物として”整理します。

- **何が発表されたのか**（UCP / Business Agent / Direct Offers）
- **「素人がつまずく点」**（“open standard”＝誰でも今すぐ同条件？ → そうとは限らない）
- **なぜ今それが必要なのか**（“探す”はできても“買う”が難しい理由）
- **他社の動き**（Visa / PayPal / Stripe / Microsoft など）

根拠は一次情報を中心に本文中へURLで埋め込みます（Google公式ブログ：`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、開発者向け解説：`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

## 3分でわかる結論（読み物版）

- **UCPは「AIが買い物を代行する」ための“共通のやり取りの型”** を作ろう、というGoogleの提案です（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。
- ただし、いきなり世界中が同日に切り替わる話ではなく、Google自身も「**eligible**」「**soon**」のように条件付きで書いています。つまり **“標準化の方向性”＋“段階的な実装”** です（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、Google実装ガイド：`https://developers.google.com/merchant/ucp/guides`）。
- 併せてGoogleは、検索でブランドと会話できる **Business Agent**（`https://support.google.com/brandprofile/answer/16410382`）や、AI Mode内で割引を出せる **Direct Offers（Google Adsのpilot）** も説明しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

## たとえ話：UCPは「通販の共通注文票」、決済は「委任状付きのレジ」

素人向けに一言でいうと、UCPはこういう発想です。

- いままで：AI（検索やチャット）が商品ページを見つけても、店ごとに購入手順が違いすぎて、**“最後のレジ”が統一できない**
- UCP：店とAIがやり取りするときの「注文票（何を買う／配送先／返品／購入後サポート等）」を**共通化** して、AIが迷わず進めるようにする

ここに「支払いの同意・証跡（あとで揉めない）」が乗ると、ようやく“代行購入”が現実味を帯びます。この“同意・証跡”の方向性はAP2の説明が分かりやすいです（`https://ap2-protocol.org/`）。

## まず事実確認：Google公式に「書いてあること」（根拠つき）

### 1) UCPは「オープン標準」として発表されたか？

GoogleはUCPを「a new open standard for agentic commerce」「open standard」「common language」などとして説明しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

また、開発者向け記事ではUCPを **open-source standard** とし、エージェント・事業者・決済事業者間の“共通言語/プリミティブ”を定義すると書いています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

### 2) discovery / buying / post-purchase support まで含むか？

Google公式ブログは「shopping journey — from discovery and buying to post-purchase support」と明示しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

UCP公式サイト側でも「discovering and buying to post purchase experiences」など、購入後の体験まで含む設計思想が示されています（`https://ucp.dev/`）。

### 3) 既存のプロトコル（A2A/AP2/MCP）と一緒に動くのか？

Google公式ブログは、UCPが既存プロトコル（Agent2Agent / Agent Payments Protocol / Model Context Protocol）と互換だと述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

開発者向け記事でも、AP2（`https://ap2-protocol.org/`）との互換、A2A（`https://a2a-protocol.org/latest/`）、MCP（`https://modelcontextprotocol.io`）やAPIでの統合方法に触れています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

補足として、A2A公式ドキュメントはA2Aを「open standard」としつつ、**Googleが開発してLinux Foundationに寄贈** した旨まで書いています（`https://a2a-protocol.org/latest/`）。

### 4) “共同開発”や“endorsed（支持）”の社名は一次情報にあるか？

Google公式ブログは、UCPが **Shopify, Etsy, Wayfair, Target, Walmart** と共同開発（co-developed）され、**American Express, Mastercard, Stripe, Visa** 等を含む「more than 20」社にendorsedされたと述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

同様の記述は開発者向け記事にもあります（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

さらに、UCPは仕様・ドキュメントがGitHubで公開されている（＝少なくとも“オープンに読める仕様”として提供されている）ことも確認できます（`https://github.com/Universal-Commerce-Protocol/ucp`、`https://ucp.dev/`）。

### 5) 「検索やGeminiの中で決済できる」は本当か？（ここが一番誤解が出やすい）

Google公式ブログは、UCPが「AI Mode in Search と Gemini app」上のチェックアウト機能を**“soon power”** し、対象が **eligible U.S. retailers** であること、支払いが **Google Pay**、さらに **PayPalもsoon** と述べています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`）。

一方で、開発者向け記事では「Google has built the first reference implementation of UCP, to power a new buying experience...」と書かれており、現時点の“まずはGoogle実装”である点も重要です（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

また、Google for Developersのガイドは「GoogleのAI surfaces（Search, Gemini）上でtransactionを可能にする」こと、導入にはMerchant Centerや承認（waitlist/approval）が絡むことを示しています（`https://developers.google.com/merchant/ucp/guides`）。

> ここは読み手の注意点：**「できるようになる」話と「誰でもすぐ使える」話は別** です。公式のキーワードは「eligible」「soon」「承認（waitlist/approval）」です（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.google.com/merchant/ucp/guides`）。

### 6) Business Agent / Direct Offers は何で、誰に関係ある？

- **Business Agent**：Google公式ブログで「launching Business Agent」として説明され、さらにBrand profileヘルプでも「Business Agent is a conversational experience on Google Search...」と定義されています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://support.google.com/brandprofile/answer/16410382`）。
- **Direct Offers**：Google公式ブログが「Introducing Direct Offers」「This new Google Ads pilot allows advertisers... directly in AI Mode」として説明しています（同URL）。

## 背景：なぜ「AIが買ってくれる」は、思ったより難しいのか

“AIが商品を探してくれる”だけなら既に多くの体験があります。問題は**「買う」** の部分です。

Visaの解説記事は、検索・比較はできても「実決済に必要な資格情報、認証・認可、信頼の枠組み」が欠けている、と整理しています（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。

Google側はもう1つのボトルネックとして、事業者（小売）側が「すべてのAIプラットフォーム/エージェントごとに個別統合」することになりがちな **N×Nの統合地獄** を挙げています（`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

ざっくり言うと、エージェントコマースは次の3点が揃わないと社会実装が進みません（＝ここが“背景”です）。

- **相互運用性**：エージェント↔店舗/プラットフォームの共通言語（UCPの狙い）
- **決済の安全性/同意**：人の同意を取り、監査可能な形で支払いを実行（AP2等の狙い）
- **ツール接続の標準化**：エージェントが外部ツール/データへ安全に接続（MCPの狙い。Microsoft Copilot StudioもMCP接続を公式に案内：`https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp`）

## ここが混ざりやすい：UCP/A2A/AP2/MCPを「役割」で整理する

Google自身がUCPの説明の中で、既存標準との関係を明示しています（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）。

ここでは“何を解決するプロトコルか”の観点で整理します。

- **UCP（Universal Commerce Protocol）**：発見〜購入〜購入後までの“商取引の流れ”を、エージェントと事業者バックエンドの間で標準化（`https://ucp.dev/`、`https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/`）
- **A2A（Agent2Agent）**：エージェント同士が安全に通信・協調するための標準（`https://a2a-protocol.org/latest/`）
- **MCP（Model Context Protocol）**：エージェントがツール/データソースに接続するための標準。MCPの仕様・スキーマがGitHubで公開されている（`https://github.com/modelcontextprotocol/modelcontextprotocol`）
- **AP2（Agent Payments Protocol）**：エージェントが支払いを行う際の安全性・責任追跡・同意証跡（non-repudiable cryptographic audit trail 等）を設計する“open protocol”（`https://ap2-protocol.org/`）

## 他社も同じ方向に動いている（だから“流行り言葉”で終わりにくい）

### 1) 決済ネットワーク側：Visaは「Visa Intelligent Commerce」を掲げる

Visaは「AI agents to buy securely and seamlessly」という文脈で「Visa Intelligent Commerce」を打ち出しています（`https://corporate.visa.com/en/products/intelligent-commerce.html`）。

より具体的な説明として、Visaのストーリー記事は「AI-ready credentials（トークン化等）」「passkey/指示・シグナル/不正検知」などを含む“信頼”の枠組みとして説明しています（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。

> 直感的に言うと、Visa側は「AIに財布を持たせるなら、**財布の作法（本人確認/同意/不正対策）** が必要だよね」という話をしています。

### 2) ウォレット/決済事業者：PayPalは“agentic commerce”を前面に出し、Mastercardと提携も

PayPalは「Mastercard and PayPal Join Forces To Accelerate Secure Global Agentic Commerce」として、Mastercard Agent PayとPayPal walletの統合に言及しています（`https://newsroom.paypal-corp.com/2025-10-27-Mastercard-and-PayPal-Join-Forces-To-Accelerate-Secure-Global-Agentic-Commerce`）。

また、同プレスリリースのメタ情報でも「agent-driven transactions」等の語が確認できます（同URL）。

### 3) 決済インフラ：Stripeは“agentic workflows”向けの公式ドキュメントを用意

Stripeは「Add Stripe to your agentic workflows」として、エージェントに決済/金融機能を組み込むためのドキュメントを公開しています（`https://docs.stripe.com/agents`）。

### 4) “エージェントにツールを与える”標準：MCPはMicrosoft Copilot Studioにも入ってきている

Microsoft Learnのドキュメントは、Copilot StudioでMCPサーバーのツール/リソースに接続してエージェントを拡張できる、と説明しています（`https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp`）。

これは、UCPのような“商取引の標準”とは別軸で、**エージェントが外部能力を安全に呼び出す標準が業界に広がっている** ことを示す材料になります。

## 報道でも同趣旨が確認できる（ただし一次情報優先）

Google公式だけでなく、以下の第三者メディアでもUCP/AI Mode/割引提示などが報じられています。

- TechCrunch: `https://techcrunch.com/2026/01/11/google-announces-a-new-protocol-to-facilitate-commerce-using-ai-agents/`
- Search Engine Land: `https://searchengineland.com/google-universal-commerce-protocol-467290`
- CNBC: `https://www.cnbc.com/2026/01/11/google-launches-universal-commerce-protocol-bets-on-ai-powered-retail.html`

（※報道は読める範囲が媒体の仕様に依存するため、**本稿の結論は一次情報を優先** しています。）

## まとめ：素人が押さえるべき“3つの注意点”

- **“open standard”＝いますぐ誰でも同条件で使える、とは限らない**：GoogleのAI Mode/Gemini側のチェックアウトは「eligible」「soon」「承認が必要」など条件がある（`https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/`、`https://developers.google.com/merchant/ucp/guides`）。
- **“エージェントが買う”の難所は「同意・責任・不正対策・資格情報」**：Visaの説明が分かりやすい（`https://www.visa.com.sg/about-visa/stories/2025/visa-intelligent-commerce-ai-agents-are-already-shopping-are-you-ready.html`）。
- **業界は分業で標準化が進んでいる**：UCP（商取引）、A2A（エージェント間）、MCP（ツール接続）、AP2（決済）という役割分担が見える（各公式URLは本文中）。

---

## もう一歩だけ深掘りしたい人へ（最短の入口リンク）

- UCP仕様：`https://ucp.dev/specification/overview/`
- Google実装ガイド：`https://developers.google.com/merchant/ucp/guides`
- AP2概要：`https://ap2-protocol.org/`
- A2A概要：`https://a2a-protocol.org/latest/`

