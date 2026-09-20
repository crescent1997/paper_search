# KV Cache — compress

既存modelのKV cacheをquantization、token selection / eviction、merging等で圧縮し、memory footprint・memory bandwidth・decode latencyを削減する論文の要約と更新履歴。

## 更新履歴

### 2026-09-20

- **[KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](2024-2402.02750-kivi.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ICML 2024）  
  KeyとValueでoutlier構造が異なることを利用し、Keyをper-channel、Valueをper-tokenで非対称2-bit量子化するtuning-free KV cache圧縮。peak memoryを2.6×削減し、batch拡大により2.35〜3.47×のthroughput向上を報告する。

- **[SnapKV: LLM Knows What You are Looking for Before Generation](2024-snapkv.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: NeurIPS 2024）  
  prompt末尾のobservation windowから各attention headがgeneration中に重視するtoken位置を推定し、重要KVだけをcluster-awareに残すfine-tuning-free compression。16K contextで8.2×のmemory efficiencyと3.6×のgeneration speedupを報告する。
