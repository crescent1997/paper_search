# RAG Security — Benchmark

RAG security / privacy benchmark・survey論文の要約と更新履歴。

## 更新履歴

### 2026-10-20

- **[LeakDojo: Decoding the Leakage Threats of RAG Systems](2026-2605.05818-leakdojo.md)** — cited: **1**（citation source: **Scholar Feed、収録範囲内の値**。Google Scholar全体の引用数ではない）  
  Private RAG knowledge baseからのretrieved-chunk leakageを6種類のattack、4種類のcorpus、複数generatorで統一比較するbenchmark。CCL≈ARC×SLTの分解とlogical maskingによるguard回避を評価。Findings of ACL 2026。

### 2026-09-10

- **[Benchmarking Knowledge-Extraction Attack and Defense on Retrieval-Augmented Generation](2026-2602.09319-benchmarking-knowledge-extraction-attack-and-defense.md)** — cited: **未確認**（citation source: **未確認**）  
  RAG knowledge-base extraction attack / defenseを複数retriever・generator・indexing方式・dataset・metricで統一比較し、retrievalとgenerationのどこで漏洩するかを分離評価するsystematic benchmark。

- **[SafeRAG: Benchmarking Security in Retrieval-Augmented Generation of Large Language Model](2025-2501.18636-saferag.md)** — cited: **未確認**（citation source: **未確認**）  
  Silver Noise、Inter-context Conflict、Soft Ad、White DoSの4種のattack scenarioを用い、retriever・filter・generatorを横断してRAG security robustnessを評価するACL 2025 benchmark。
