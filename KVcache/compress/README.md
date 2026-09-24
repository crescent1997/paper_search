# KV Cache — compress

既存modelのKV cacheをquantization、token selection / eviction、merging等で圧縮し、memory footprint・memory bandwidth・decode latencyを削減する論文の要約と更新履歴。

## 更新履歴

### 2026-09-24

- **[Scissorhands: Exploiting the Persistence of Importance Hypothesis for LLM KV Cache Compression at Test Time](2023-2305.17118-scissorhands.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: NeurIPS 2023）  
  過去のattentionで重要だったtokenは将来も重要であり続けやすいというPersistence of Importanceに基づき、recent tokenを保護しつつpivotal KVを保持するtraining-free online eviction。最大約5×、4-bit quantization併用で最大約20×のKV compressionを報告する。

- **[Efficient Streaming Language Models with Attention Sinks](2023-2309.17453-streamingllm.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: ICLR 2024）  
  sliding windowで初期tokenを捨てると性能が崩れるattention sink現象を分析し、少数のinitial sink tokenとrecent windowだけを保持する固定KV budgetのstreaming inferenceを提案。4M+ tokenのstreamingと最大22.2×のspeedupを報告する。

### 2026-09-22

- **[H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](2023-2306.14048-h2o.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: NeurIPS 2023）  
  累積attention scoreの高いHeavy Hitterとrecent tokenを優先保持するdynamic KV eviction。20% heavy-hitter設定でFlexGen比最大3×、DeepSpeed Zero-Inference / HF Accelerate比最大29×のthroughputを報告し、後続attention-based KV compressionの基点となった。

### 2026-09-20

- **[KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](2024-2402.02750-kivi.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ICML 2024）  
  KeyとValueでoutlier構造が異なることを利用し、Keyをper-channel、Valueをper-tokenで非対称2-bit量子化するtuning-free KV cache圧縮。peak memoryを2.6×削減し、batch拡大により2.35〜3.47×のthroughput向上を報告する。

- **[SnapKV: LLM Knows What You are Looking for Before Generation](2024-snapkv.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: NeurIPS 2024）  
  prompt末尾のobservation windowから各attention headがgeneration中に重視するtoken位置を推定し、重要KVだけをcluster-awareに残すfine-tuning-free compression。16K contextで8.2×のmemory efficiencyと3.6×のgeneration speedupを報告する。
