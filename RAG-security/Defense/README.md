# RAG Security — Defense

RAG security / privacy defense論文の要約と更新履歴。

## 更新履歴

### 2026-09-22

- **[PRA-RAG: Provably Robust Aggregation in Retrieval-Augmented Generation against Retrieval Corruption](2026-2607.00012-pra-rag.md)** — cited: **未確認**（citation source: **未確認**）  
  document subsetsのsemantic representationsからmajority minimum-radius ballを求め、weighted robust aggregationでpoisoned retrievalの影響を抑える。representation deviationの理論boundとPADを与える。Findings of ACL 2026。

### 2026-09-20

- **[RAGSentinel: Certifiable Geometric Consensus for Robust Retrieval-Augmented Generation](2026-2608.23965-ragsentinel.md)** — cited: **未確認**（citation source: **未確認**）  
  surrogate encoderのquery-conditioned hidden-state residualをgeometric consensusで評価し、poisoned retrieved documentsをgeneration前に除去する。honest-majority / separation条件下でpoison-free contextのcertifiable filtering条件も与える。EMNLP 2026 Main Conference。

- **[TRIS: A Tri-Layer Retrieval Integrity Sieve Against Knowledge Poisoning](2026-2609.00470-tris.md)** — cited: **未確認**（citation source: **未確認**）  
  independent embedding geometry、trigger/payload structural detection、LLM parametric-belief verificationを組み合わせ、PoisonedRAG型knowledge poisoningを低costなadaptive middlewareでfilterする。Findings of EMNLP 2026。

### 2026-09-17

- **[PRAG: End-to-End Privacy-Preserving Retrieval-Augmented Generation](2026-2604.26525-prag.md)** — cited: **2**（citation source: **alphaXiv**）  
  CKKS homomorphic encryption上でdocument/query embeddingsとANN retrievalを秘匿し、encrypted K-means + HNSWでcloud-side semantic retrievalを高速化する。さらにOEEでranking errorを制御し、dummy traversalとperiodic rebuildでHNSW access-pattern leakageも抑える。

### 2026-09-15

- **[Privacy-Preserving Retrieval-Augmented Generation with Differential Privacy](2024-2412.04697-privacy-preserving-rag-differential-privacy.md)** — cited: **16**（citation source: **Papersgraph**）  
  retrieved documentsをdisjoint groupsへ分割したLLM votingにDifferential Privacyを適用し、DPSparseVoteRAGではprivate knowledgeが不要なtokenのprivacy-budget消費を回避してdocument-level privacyと生成utilityを両立する。

- **[SD-RAG: A Prompt-Injection-Resilient Framework for Selective Disclosure in Retrieval-Augmented Generation](2026-2601.11199-sd-rag.md)** — cited: **3**（citation source: **OUCI**）  
  user queryから隔離したredaction stageでretrieved private contextを先にsanitizeし、natural-language privacy constraintsに基づくselective disclosureによってprompt injection成功時のsensitive-context leakageを抑える。

### 2026-09-14

- **[Mitigating the Privacy Issues in Retrieval-Augmented Generation (RAG) via Pure Synthetic Data](2024-2406.14773-sage.md)** — cited: **7**（citation source: **Lune**）  
  private corpusをattribute-based generationとagent-based iterative privacy refinementでsynthetic corpusへ置換し、RAG utilityを維持しながらtargeted / untargeted data extraction leakageを大幅に抑えるSAGE。EMNLP 2025 Main。

- **[Pisces: Cryptography-based Private Retrieval-Augmented Generation with Dual-Path Retrieval](2026-pisces-private-rag.md)** — cited: **未確認**（citation source: **未確認**）  
  semantic retrievalとlexical retrievalの双方をMPC / PIR / FHE / PSI等でprivateに実行し、user queryとserver-side knowledge baseを秘匿したままhybrid RAG retrievalを行うcryptographic framework。ICLR 2026。

### 2026-09-13

- **[Detecting RAG Extraction Attack via Dual-Path Runtime Integrity Game](2026-2604.10717-canaryrag.md)** — cited: **0**（citation source: **ResearchGate**）  
  retrieved chunksへcanary tokensを埋め込み、Target / Oracleのdual-pathで相反するintegrity条件を監視することで、adaptiveなknowledge-base extractionをruntimeで検知・停止するCanaryRAG。ACL 2026 Main Conference。

### 2026-09-11

- **[RAGFort: Dual-Path Defense Against Proprietary Knowledge Base Extraction in Retrieval-Augmented Generation](2025-2511.10128-ragfort.md)** — cited: **1**（citation source: **ResearchGate**）  
  knowledge-base extractionをinter-class explorationとintra-class exploitationに分解し、contrastive reindexingとconstrained cascade generationで両経路を抑えるdual-path defense。AAAI 2026。

### 2026-09-09

- **[Differentially Private Synthetic Text Generation for Retrieval-Augmented Generation (RAG)](2025-2510.06719-dp-synrag.md)** — cited: **未確認**（citation source: **未確認**）  
  private corpusをone-timeのDP synthetic corpusへ変換し、その後のqueryでは追加privacy budgetを消費せず通常のRAGとして利用するdata-layer defenseを提案する。
