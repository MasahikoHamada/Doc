---
title: "LLMは「覚える」より「引く」？ DeepSeek×北大のEngram（条件付きメモリ）をやさしく読む"
tags:
  - LLM
  - 論文紹介
  - DeepSeek
  - arXiv
  - AIエージェント
private: false
updated_at: "2026-01-14"
---

## はじめに：この論文は何が新しい？

ChatGPTのような大規模言語モデル（LLM）は、知識を持っているように見えて、実際には「知っている内容を毎回“計算で再構成”」して答えている場面が多くあります。  
この論文はそこに問題意識を置き、**“知識の参照（lookup）”をモデルの基本部品として持たせよう**という提案です。

対象は、DeepSeek と Peking University（北京大学）の研究者らによる **Engram**。論文タイトルは *Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models* です（arXiv: `https://arxiv.org/abs/2601.07372`）。

## 先に結論：Engramを一言で言うと

Engramは、Transformerが苦手な「知識の参照」を、**ハッシュ化したN-gram埋め込みの O(1) 参照**として外付けし、  
そのぶん**本体（バックボーン）の計算資源を“推論”や“長文の文脈理解”に寄せる**、という設計です（要約は論文アブストラクトに明記：`https://arxiv.org/abs/2601.07372`）。

## たとえ話：脳内検索ではなく「付箋インデックス」を作る

- **従来のLLM**：分厚い教科書を暗記して、必要な情報を“思い出す”  
- **Engramの発想**：よく出るパターン（定型フレーズや局所依存）を、**付箋（インデックス）で一瞬で引ける**ようにしておく

付箋を引くのは高速なので、脳（ニューラル計算）は難問に集中できる、というイメージです。

## 何をしているの？（できるだけ平易に）

論文が言っているコアを、素人向けに噛み砕くと次の3点です。

### 1) 「知識参照の部品」がないのがTransformerの弱点だ、という問題設定

MoE（Mixture-of-Experts）は「計算を条件付きで使い分ける」ことでモデル容量を伸ばしますが、Transformerには「知識を引く」ためのネイティブなプリミティブがなく、参照を計算で“擬似的に再現”しがちだ、と述べています（アブストラクト：`https://arxiv.org/abs/2601.07372`）。

### 2) Engram＝ハッシュ化した N-gram 埋め込みを O(1) で引く「条件付きメモリ」

Engramは古典的な N-gram を現代的に作り直し、**O(1) lookup**で静的パターンを参照できるようにする、と要約されています（アブストラクト：`https://arxiv.org/abs/2601.07372`）。  
arXivのHTML版でも「Sparse Retrieval via Hashed N-grams」という節があり、ハッシュN-gramの方向性が確認できます（`https://arxiv.org/html/2601.07372v1`）。

### 3) “早い層”を雑務から解放し、Attentionを長距離文脈に回す（という解釈）

著者らはメカニスティック分析として、Engramが**初期層を静的再構成（static reconstruction）から解放し、より複雑な推論に使えるようにする**、また**局所依存をlookupに委ねることでattentionがグローバル文脈に集中できる**と説明しています（アブストラクト：`https://arxiv.org/abs/2601.07372`、まとめ部の記述：`https://arxiv.org/html/2601.07372v1`）。

## 本当に速いの？（推論時の“ホストメモリ退避”とオーバーヘッド）

この論文が面白いのは「大きなテーブル＝遅い」を真正面から扱っている点です。

arXiv HTML版の **System Efficiency** の節では、Engramの参照が**静的ハッシュIDで決まるため、次に必要な参照先を前もって計算でき、prefetchできる**（＝通信と計算のオーバーラップができる）と説明されています（`https://arxiv.org/html/2601.07372v1`）。

さらに、**100Bパラメータの埋め込みテーブルをホストメモリへ完全にオフロード**した場合でも、スループット低下は最大で **2.8%** と書かれています（Table 4 周辺：`https://arxiv.org/html/2601.07372v1`）。  
ユーザー文の「3%未満」は、この **2.8%** を指していると読むのが自然です。

## どれくらい強くなったの？（論文が主張している改善幅）

論文アブストラクトには、**厳密に“同一パラメータ数・同一FLOPs”のMoEベースライン**に対して、Engramを **27B parameters** までスケールした結果が要約されています（`https://arxiv.org/abs/2601.07372`）。

記載されている改善例（抜粋）は以下です。

- **知識系**: MMLU +3.4、CMMLU +4.0  
- **推論系**: BBH +5.0、ARC-Challenge +3.7  
- **コード/数学**: HumanEval +3.0、MATH +2.4  
- **長文検索**: Multi-Query NIAH 84.2 → 97.0

（いずれもアブストラクトに明記：`https://arxiv.org/abs/2601.07372`）

## “すごそう”の裏で、読み手が注意すべき点

この手の論文は、誤解が生まれやすいポイントもあります。少なくとも次は切り分けて読むのが安全です。

- **「静的メモリ（lookupできる表）」と「推論能力」**は別物：論文の主張は「静的パターンはlookupに任せ、推論に計算資源を回す」こと（`https://arxiv.org/abs/2601.07372`）
- **“open-source”の範囲**：DeepSeek側はEngramの公式実装リポジトリを公開しており、デモコードもあります（`https://github.com/deepseek-ai/Engram`）。一方で、実際の学習/推論環境を自分で再現するには追加要素が要る可能性があります（READMEの注意書きも参照：`https://github.com/deepseek-ai/Engram/blob/main/README.md`）
- **性能改善はタスク/設定依存**：アブストラクトの数字は“例”で、詳細条件は本文の実験設定を確認すべきです（arXiv PDF：`https://arxiv.org/pdf/2601.07372`、HTML：`https://arxiv.org/html/2601.07372v1`）

## 参考リンク（一次情報）

- arXiv（アブストラクト）: `https://arxiv.org/abs/2601.07372`
- arXiv（PDF）: `https://arxiv.org/pdf/2601.07372`
- arXiv（HTML）: `https://arxiv.org/html/2601.07372v1`
- 公式実装（DeepSeek）: `https://github.com/deepseek-ai/Engram`

