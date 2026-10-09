# KV Cache — model

attention / decoder architectureそのものを変更し、KV head数、cached representationの次元、cacheを保持するlayer数・回数を構造的に減らす論文の要約と更新履歴。

## 更新履歴

### 2026-10-09

- **★ [DeepSeek-V2: Multi-head Latent Attention (MLA)](2024-2405.04434-deepseek-v2-mla.md)** — cited: **1,411**（Scholar Feed、前回調査値）/ arXiv technical report 2024。joint low-rank KV latent、decoupled RoPE、projection absorptionでcache量を構造的に削減。

### 2026-10-08

- **[Layer-Condensed KV Cache (LCKV)](2024-lckv.md)** — cited: **23（Semantic Scholar系二次集計）** / **ACL 2024 Main**。upper-layer KVを複数layerから共有する構造的削減。
- **[A Systematic Study of Cross-Layer KV Sharing](2025-cross-layer-kv-sharing-systematic.md)** — cited: **17（Semantic Scholar系二次集計）** / **NAACL 2025 Short**。LCKV/CLA/YOCO等をsharing topologyで統一比較。

### 2026-10-07

- **[KVSharer: Efficient Inference via Layer-Wise Dissimilar KV Cache Sharing](2024-2410.18517-kvsharer.md)** — cited: **39**（citation source: Scholar Feed; status: arXiv / Under Review by ICLR 2025）  
  pretrained Transformerのselected layers間でKV cacheを共有し、約30%のKV computation削減と1.3×以上のgeneration speedupを報告する。CLA等のarchitecture-level sharingとは異なり、既存modelのlayer redundancyを利用する。

### 2026-10-04

- **[Reconstructing KV Caches with Cross-Layer Fusion for Enhanced Transformers (FusedKV)](2025-2512.03870-fusedkv.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: ICLR 2026）  
  Valueはbottom layer、Keyはbottom/middle layerから強く情報を継承する非対称性を利用してtop-layer KVを再構成する。332M–4BでKV cache memoryを50%削減しつつstandard Transformerより低いvalidation perplexityを報告する。

### 2026-10-02

- **★ [Reducing Transformer Key-Value Cache Size with Cross-Layer Attention](2024-2405.12981-cross-layer-attention.md)** — cited: **131**（citation source: Scholar Feed; venue: NeurIPS 2024 Main）  
  MQA/GQAのhead-wise KV sharingをlayer方向へ拡張し、隣接layer間でK/V activationsを共有するarchitecture-level reduction。CLA2ではMQA比でKV cacheをさらに約2×削減しつつ、ほぼ同等のaccuracyを維持する。

### 2026-09-24

- **[GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints](2023-2305.13245-gqa.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: EMNLP 2023 Main）  
  query headsをgroup化し、group内でK/V headを共有するGeneralized Multi-Query Attention。MHAに近い品質を維持しながらKV head数を構造的に削減し、MQAに近いdecode効率を狙う。既存MHA checkpointから元pretraining computeの約5%でuptraining可能。

### 2026-09-20

- **[You Only Cache Once: Decoder-Decoder Architectures for Language Models](2024-2405.05254-yoco.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: NeurIPS 2024）  
  self-decoderが一度だけglobal KV cacheを作り、後段cross-decoderの全layerがそれを共有するdecoder-decoder architecture。長文でlayer数に比例して増えるKV cacheを構造的に抑える。

- **[Towards Economical Inference: Enabling DeepSeek’s Multi-Head Latent Attention in Any Transformer-based LLMs](2025-2502.14837-mha2mla.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ACL 2025 Main）  
  MHA modelをpartial-RoPEとjoint SVDでMLAへ変換するMHA2MLA。Llama2-7BでKV cacheを92.19%削減しつつ、少量の追加学習でLongBench性能をほぼ回復する。
