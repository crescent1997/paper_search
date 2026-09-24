# KV Cache — model

attention / decoder architectureそのものを変更し、KV head数、cached representationの次元、cacheを保持するlayer数・回数を構造的に減らす論文の要約と更新履歴。

## 更新履歴

### 2026-09-24

- **[GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints](2023-2305.13245-gqa.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: EMNLP 2023 Main）  
  query headsをgroup化し、group内でK/V headを共有するGeneralized Multi-Query Attention。MHAに近い品質を維持しながらKV head数を構造的に削減し、MQAに近いdecode効率を狙う。既存MHA checkpointから元pretraining computeの約5%でuptraining可能。

### 2026-09-20

- **[You Only Cache Once: Decoder-Decoder Architectures for Language Models](2024-2405.05254-yoco.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: NeurIPS 2024）  
  self-decoderが一度だけglobal KV cacheを作り、後段cross-decoderの全layerがそれを共有するdecoder-decoder architecture。長文でlayer数に比例して増えるKV cacheを構造的に抑える。

- **[Towards Economical Inference: Enabling DeepSeek’s Multi-Head Latent Attention in Any Transformer-based LLMs](2025-2502.14837-mha2mla.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ACL 2025 Main）  
  MHA modelをpartial-RoPEとjoint SVDでMLAへ変換するMHA2MLA。Llama2-7BでKV cacheを92.19%削減しつつ、少量の追加学習でLongBench性能をほぼ回復する。
