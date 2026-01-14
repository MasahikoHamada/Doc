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
この論文はそこに問題意識を置き、**“知識の参照（lookup）”をモデルの基本部品として持たせようという提案です**。

対象は、DeepSeek と Peking University（北京大学）の研究者らによる **Engram**。論文タイトルは *Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models* です（arXiv: `https://arxiv.org/abs/2601.07372`）。

## 先に結論：Engramを一言で言うと

Engramは、Transformerが苦手な「知識の参照」を、**ハッシュ化したN-gram埋め込みの O(1) 参照として外付けし**、  
そのぶん**本体（バックボーン）の計算資源を“推論”や“長文の文脈理解”に寄せる**、という設計です（要約は論文アブストラクトに明記：`https://arxiv.org/abs/2601.07372`）。

## たとえ話：脳内検索ではなく「付箋インデックス」を作る

- **従来のLLM**：分厚い教科書を暗記して、必要な情報を“思い出す”  
- **Engramの発想**：よく出るパターン（定型フレーズや局所依存）を、**付箋（インデックス）で一瞬で引けるようにしておく**

付箋を引くのは高速なので、脳（ニューラル計算）は難問に集中できる、というイメージです。

## 何をしているの？（できるだけ平易に）

論文が言っているコアを、素人向けに噛み砕くと次の3点です。

### 1) 「知識参照の部品」がないのがTransformerの弱点だ、という問題設定

MoE（Mixture-of-Experts）は「計算を条件付きで使い分ける」ことでモデル容量を伸ばしますが、Transformerには「知識を引く」ためのネイティブなプリミティブがなく、参照を計算で“擬似的に再現”しがちだ、と述べています（アブストラクト：`https://arxiv.org/abs/2601.07372`）。

### 2) Engram＝ハッシュ化した N-gram 埋め込みを O(1) で引く「条件付きメモリ」

Engramは古典的な N-gram を現代的に作り直し、**O(1) lookup** で静的パターンを参照できるようにする、と要約されています（アブストラクト：`https://arxiv.org/abs/2601.07372`）。  
arXivのHTML版でも「Sparse Retrieval via Hashed N-grams」という節があり、ハッシュN-gramの方向性が確認できます（`https://arxiv.org/html/2601.07372v1`）。

### 3) “早い層”を雑務から解放し、Attentionを長距離文脈に回す（という解釈）

著者らはメカニスティック分析として、Engramが**初期層を静的再構成（static reconstruction）から解放し、より複雑な推論に使えるようにする**、また**局所依存をlookupに委ねることでattentionがグローバル文脈に集中できる** と説明しています（アブストラクト：`https://arxiv.org/abs/2601.07372`、まとめ部の記述：`https://arxiv.org/html/2601.07372v1`）。

## 本当に速いの？（推論時の“ホストメモリ退避”とオーバーヘッド）

この論文が面白いのは「大きなテーブル＝遅い」を真正面から扱っている点です。

arXiv HTML版の **System Efficiency** の節では、Engramの参照が**静的ハッシュIDで決まるため、次に必要な参照先を前もって計算でき、prefetchできる**（＝通信と計算のオーバーラップができる）と説明されています（`https://arxiv.org/html/2601.07372v1`）。

さらに、**100Bパラメータの埋め込みテーブルをホストメモリへ完全にオフロードした場合でも**、スループット低下は最大で **2.8%** と書かれています（Table 4 周辺：`https://arxiv.org/html/2601.07372v1`）。  
ユーザー文の「3%未満」は、この **2.8%** を指していると読むのが自然です。

## どれくらい強くなったの？（論文が主張している改善幅）

論文アブストラクトには、**厳密に“同一パラメータ数・同一FLOPs”のMoEベースラインに対して**、Engramを **27B parameters** までスケールした結果が要約されています（`https://arxiv.org/abs/2601.07372`）。

記載されている改善例（抜粋）は以下です。

- **知識系**: MMLU +3.4、CMMLU +4.0  
- **推論系**: BBH +5.0、ARC-Challenge +3.7  
- **コード/数学**: HumanEval +3.0、MATH +2.4  
- **長文検索**: Multi-Query NIAH 84.2 → 97.0

（いずれもアブストラクトに明記：`https://arxiv.org/abs/2601.07372`）

## “すごそう”の裏で、読み手が注意すべき点

この手の論文は、誤解が生まれやすいポイントもあります。少なくとも次は切り分けて読むのが安全です。

- **「静的メモリ（lookupできる表）」と「推論能力」は別物**：論文の主張は「静的パターンはlookupに任せ、推論に計算資源を回す」こと（`https://arxiv.org/abs/2601.07372`）
- **“open-source”の範囲**：DeepSeek側はEngramの公式実装リポジトリを公開しており、デモコードもあります（`https://github.com/deepseek-ai/Engram`）。一方で、実際の学習/推論環境を自分で再現するには追加要素が要る可能性があります（READMEの注意書きも参照：`https://github.com/deepseek-ai/Engram/blob/main/README.md`）
- **性能改善はタスク/設定依存**：アブストラクトの数字は“例”で、詳細条件は本文の実験設定を確認すべきです（arXiv PDF：`https://arxiv.org/pdf/2601.07372`、HTML：`https://arxiv.org/html/2601.07372v1`）

## 競合（研究）の現況：Engramはどこに位置づく？

ここから先は、Engramと“同じ問題（知識/記憶）を別のやり方で解く”系の代表例を、ざっくり地図化します。  
（※「競合他社」を“企業プロダクトの勝ち負け”として断定するのは難しいため、**研究アプローチの競合として整理します**。）

### A) いちばん普及している路線：外部検索＋生成（RAG）

**RAG（Retrieval-Augmented Generation）** は「外部の知識ベースから検索して、その結果をもとに生成する」路線です（論文：`https://arxiv.org/abs/2005.11401`）。知識更新がしやすく、運用で採用されやすいのが強みです。

Engramとの違いは、RAGが **外部コーパス（文書）** を引くのに対して、Engramは **モデル内部の“静的パターン表（embedding table）”** を引く点です（`https://arxiv.org/abs/2601.07372`）。

### B) “学習時から検索込み”の大型路線：RETRO（Retrieval-Enhanced Transformer）

DeepMindのRETROは「trillions of tokensから検索して性能を上げる」方向性として有名です（論文：`https://arxiv.org/abs/2112.04426`、解説ブログ：`https://deepmind.google/blog/improving-language-models-by-retrieving-from-trillions-of-tokens/`）。

Engramのような**O(1)のテーブル参照** とは別に、RETROは大規模検索（近傍検索）を扱うため、システム要件や運用コストの設計が別物になりがちです。

### C) “推論時に最近傍を足す”路線：kNN-LM（Nearest Neighbor Language Models）

kNN-LMは、学習済みモデルの内部表現（例：中間層表現）を巨大データストアに保存しておき、推論時に最近傍を引いて補正する発想です（`https://arxiv.org/abs/1911.00172`）。

Engramは「静的ハッシュ参照」で**アクセスパターンを決定的にし、prefetchでオーバーヘッドを抑える** ことにフォーカスしています（`https://arxiv.org/html/2601.07372v1`）。この点は、kNN系の“検索そのもの”のコスト構造と対照的です。

### D) “モデルの中に巨大メモリ層を置く”路線：Product Key Memory（PKM）

Product Key Memoryは、キーの組み合わせで巨大メモリを参照することで容量を稼ぐ系のアプローチです（`https://arxiv.org/abs/1907.05242`）。

近年もPKMの派生・更新は続いており、「Fast-weight Product Key Memory」として新しい提案も出ています（`https://arxiv.org/abs/2601.00671`）。

EngramとPKMはどちらも「計算を増やさずに容量を増やす」方向ですが、Engramは**N-gram＋ハッシュ参照** に寄せた設計で、論文内では「静的パターンをlookupへ逃がす」解釈を強調しています（`https://arxiv.org/abs/2601.07372`）。

### E) “外部メモリを読めるTransformer”路線：Memorizing Transformers

Memorizing Transformersは、Transformerに外部メモリを持たせる方向性の代表例です（`https://arxiv.org/abs/2203.08913`）。

Engramが示しているのは「アクセスパターンを決定的にしてprefetchし、ホストメモリ退避でもオーバーヘッドを小さくできる」という**システム設計の論点** でもあります（`https://arxiv.org/html/2601.07372v1`）。

### F) “ハッシュ埋め込み”という古くて強い基礎（fastText / Hash Embeddings）

Engramの技術要素は「突然現れた魔法」ではなく、N-gram/サブワードやハッシュ埋め込みの系譜の上にあります。

- fastText（サブワード情報で単語ベクトルを強化）: `https://arxiv.org/abs/1607.04606`
- Hash Embeddings（ハッシュで語彙表現を効率化）: `https://arxiv.org/abs/1709.03933`

Engramはこれらを「LLMの中で、スケールする“条件付きメモリ”として再構成した」位置づけだと読むのが分かりやすいです（`https://arxiv.org/abs/2601.07372`）。

## 参考：各社はどの手法でLLMを改良している？（公開情報ベース）

本稿はEngram中心ですが、比較のために「大手が“公開情報として”語っている改良の型」も軽く触れておきます。  
（※企業の内部実装の詳細は非公開が多いので、**一次情報で確認できる範囲** に限定します。）

- **OpenAI**：人間フィードバックでの整列（例：InstructGPT のRLHF枠組み：`https://arxiv.org/abs/2203.02155`）と、ツール利用（function calling：`https://platform.openai.com/docs/guides/function-calling`、retrieval：`https://platform.openai.com/docs/guides/retrieval`）
- **Google（Gemini）**：スケール＋システム最適化（Gemini Technical Report：`https://arxiv.org/abs/2312.11805`）と、ツール利用（function calling：`https://ai.google.dev/gemini-api/docs/function-calling`）
- **Anthropic（Claude）**：Constitutional AI による整列（`https://arxiv.org/abs/2212.08073`）と、ツール利用（tool use：`https://docs.anthropic.com/en/docs/tool-use/overview`）
- **Meta（Llama）**：オープンな学習/整列の枠組み（Llama 2 論文：`https://arxiv.org/abs/2307.09288`）。検索/メモリを外付けして補う研究系譜（例：kNN-LM：`https://arxiv.org/abs/1911.00172`）も強い
- **DeepSeek（Engram文脈）**：条件付きメモリ（lookup）で“計算以外の軸”を足す（Engram：`https://arxiv.org/abs/2601.07372`、実装：`https://github.com/deepseek-ai/Engram`）

ざっくり言うと、業界全体は **(1) 整列（RLHF等）(2) ツール/検索の統合（RAG等）(3) 計算・メモリ・システムの効率化（MoE/lookup等）** の組み合わせで改良している、という見立てになります。

## 今後の予測（不確実性つき）

ここからは一次情報ではなく、上の潮流を踏まえた**予測です**（外れる可能性があります）。

### 短期（〜1年）：RAG優位は続きつつ、ハイブリッド化が進む

- **運用面では**、知識更新・検証・監査のしやすさから、RAG（`https://arxiv.org/abs/2005.11401`）系が引き続き主流になりやすいです。
- **研究/実装面では**、Engramが示した「決定的アクセス＋prefetch＋ホストメモリ退避で2.8%程度」という数字（`https://arxiv.org/html/2601.07372v1`）がインパクトを持ち、**“計算量を増やさず容量を盛る”系の実装が増える可能性があります**。

### 中期（1〜2年）：MoE（計算の疎性）×メモリ（参照の疎性）×検索（外部RAG）の“三層構え”へ

Engram論文自身が「MoE（条件付き計算）＋Engram（条件付きメモリ）」という“2軸の疎性”を提案しているため（`https://arxiv.org/abs/2601.07372`）、今後は

- **局所パターン**：ハッシュ/埋め込みテーブルでO(1)参照（Engram系）
- **最新知識/根拠提示**：外部RAG（RAG/RETRO系）
- **重い推論**：バックボーン（MoE含む）

のように、役割分担する設計が現実的になっていきます。

### 長期（2年〜）：鍵は「安全に更新できるか」「ハードウェアと一緒に最適化できるか」

Engram型の“静的メモリ”は高速ですが、運用では次が論点になります。

- **更新**：静的テーブルをどう更新・配布するか（モデル更新より軽いのか、結局重いのか）
- **安全性**：誤った静的パターン（古い/偏った/汚染された）を引くリスク
- **システム最適化**：決定的アクセスを活かしたprefetchやキャッシュ戦略を、どこまで一般化できるか（`https://arxiv.org/html/2601.07372v1`）

「速いが固定されがち」なメモリと、「更新しやすいが検索が重い」RAGの間で、**どこをどの方式に任せるかが勝ち筋になります**。

## 参考リンク（一次情報）

- arXiv（アブストラクト）: `https://arxiv.org/abs/2601.07372`
- arXiv（PDF）: `https://arxiv.org/pdf/2601.07372`
- arXiv（HTML）: `https://arxiv.org/html/2601.07372v1`
- 公式実装（DeepSeek）: `https://github.com/deepseek-ai/Engram`

