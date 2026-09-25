# KV Cache — access

decode時にKV cache全体を毎step読み込むのではなく、query-aware selection、page/block retrieval、head specialization、CPU/GPU hierarchy等によって**実際にread / transferするKV量を減らし、memory-bandwidth bottleneckを緩和する手法**の要約と更新履歴。

`compress` が保存されるKV cache自体の容量削減を主眼とするのに対し、`access` はKVを保持したままでもdecode時のread setを疎にすることを主眼とする。

## Scope

- **Selective KV fetch / sparse attention** — queryごとに重要token / page / blockだけを選び、decode時にそのKVだけ読む。
- **Hierarchical KV access** — GPU HBMに全KVを置かず、CPU / host memory等から必要部分だけretrieveする。
- **Head-specialized access** — retrieval headだけfull KVを読み、streaming / local headは小さいcacheだけを読む。
- **Page / block level indexing** — KV cacheをpage/block単位で索引化し、query-dependentに候補を絞る。
- **Vector-retrieval-based KV access** — attention候補をANN / vector searchで事前選択し、full attentionのread量を削減する。

## Boundary with other categories

- `compress`: 保存KVのbit数、token数、head数等を削減する。
- `model`: architecture変更によってそもそも生成されるKV量を減らす。
- `reuse`: 別request / documentで既存KVを再利用してprefillを削減する。
- `access`: **decode stepごとに実際に読み込むKV量を減らす**。

1本の論文が複数カテゴリにまたがる場合は、主たる性能改善要因に基づいて配置し、関連カテゴリREADMEから相対リンクする。

## 更新履歴

### 2026-09-25

- **[SparQ Attention: Bandwidth-Efficient LLM Inference](2023-2312.04985-sparq-attention.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: ICML 2024）  
  current queryの大きなdimensionだけで全Keyを粗くscoreし、上位tokenの完全なK/Vだけをfetchするquery-aware sparse attention。KV自体は保持したままattention data transferを最大8×削減し、GPU最大4.17×、CPU 2.5×、IPU 7.41×のattention speedupを報告する。

## Historical backfill candidates

以下は優先的に精査する。

- Quest (arXiv:2406.10774)
- RetrievalAttention (arXiv:2409.10516)
- DuoAttention (arXiv:2410.10819)
- RetroInfer (arXiv:2505.02922)

候補選定時は、KV cache容量だけでなく、1 decode stepで実際に何token / page / head / layer分を読むか、HBM/CPU transfer量、decode latency / throughput、selection/indexing overheadを重点的に確認する。
