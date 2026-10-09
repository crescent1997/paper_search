# KV Cache — reuse

異なるrequest / document / prompt module間でprecomputed KV cacheを再利用し、prefill計算とTTFTを削減する論文の要約と更新履歴。推論時のKV再配置・部分再計算だけでなく、**KV再利用を前提にfine-tuningしてモデルを適応させる手法**も対象とする。

## 更新履歴

### 2026-10-09

- **[LinearKV: One Cached State Suffices for Position-Independent Caching in Hybrid LLMs](2026-2608.11231-linearkv.md)** — ユーザー指定・citation基準免除（引用数未確認）/ arXiv 2026。Hybrid LLMで単一cached recurrent stateを使い、Mamba-2のcomposition誤差を回避する。
- **[HYPIC: Accelerating Hybrid-Attention LLM Serving with Position-Independent Caching](2026-2607.01299-hypic.md)** — ユーザー指定・citation基準免除（引用数未確認）/ arXiv 2026。transition-aware state composition、seam recomputation、cold-segment並列prefill。

### 2026-10-08

- **[TurboRAG](2025-turborag.md)** — cited: **7（secondary source、直接検証未完）** / venue fallback: **EMNLP 2025 Main**。offline chunk KV precomputationとindependent attention / reordered RoPEによるRAG TTFT短縮。平均8.6×、最大9.4×を報告。

### 2026-10-07

- **★ [RAGCache: Efficient Knowledge Caching for Retrieval-Augmented Generation](2024-2404.12457-ragcache.md)** — cited: **145**（citation source: Scholar Feed; publication: ACM TOCS）  
  prefix-sensitiveなknowledge treeでRAG document KVをcross-request reuseし、GPU/host階層cache、PGDSF eviction、cache-aware schedulingを組み合わせる。vLLM+Faiss比でTTFT最大4×、throughput最大2.1×改善。

- **[Faster and Longer Context-Augmented Generation via Adaptive Parallel Encoding (APE)](2025-2502.05431-ape.md)** — cited: **未確認**（citation source: 信頼値未取得; venue fallback: ICLR 2025）  
  independent parallel encodingのattention distribution差をshared prefix、temperature、scalingで補正し、selective recomputationなしでsequential encodingに近い品質を狙うtraining-free手法。

### 2026-10-06

- **★ [Prompt Cache: Modular Attention Reuse for Low-Latency Inference](2023-2311.04934-prompt-cache.md)** — cited: **320**（citation source: Scholar Feed; venue: MLSys 2024）  
  prompt moduleとschema-defined logical positionにより、prefix完全一致に限定されないmodular KV reuseを実現する初期重要研究。model変更なしでGPU最大8×、CPU最大60×のTTFT改善を報告する。

- **[Cache-Craft: Managing Chunk-Caches for Efficient Retrieval-Augmented Generation](2025-2502.15734-cache-craft.md)** — cited: **未確認**（citation source: 信頼値未取得; venue fallback: SIGMOD 2025）  
  RAG chunk-cacheのcontext dependencyを推定し、selected tokenだけを再計算してreuse品質を修復。cache management、I/O overlap、scattered-query向けTriton attentionまで含むend-to-end system。

- **★ [KVLink: Accelerating Large Language Models via Efficient KV Cache Reuse](2025-2502.16002-kvlink.md)** — cited: **75**（citation source: Scholar Feed; venue: NeurIPS 2025 Main）  
  independently prefetched document KVのpositionを補正し、失われるcross-document interactionをtrainable link tokensで補うreuse-aware fine-tuning。standard inference比で最大96%のTTFT削減を報告する。

### 2026-10-04

- **[RelayCaching: Accelerating LLM Collaboration via Decoding KV Cache Reuse](2026-2603.13289-relaycaching.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: ICML 2026）  
  upstream agentのdecode KVをdownstream agentのprefillへrelayし、prefix差による局所的deviationだけをlayer/token単位でrepairする。80%以上のKV reuseと最大4.7×のTTFT短縮を報告する。

- **[C²KV: Compressed and Composable KV Cache Reuse for Efficient LLM Inference](2026-2607.17715-c2kv.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: KDD 2026）  
  lightweight sidecar extractorでcompressed・position-agnostic・composableなreuse representationを学習し、full-size KVのstorage/transfer bottleneckも削減する。長contextで最大17×のinference speedupを報告する。

### 2026-10-03

- **[SpecCache: Speculative KV Cache Reuse for Efficient RAG Serving](2026-speccache.md)** — cited: **未確認**（citation source: Google Scholar信頼値未取得; venue fallback: ACL 2026 Main / Long Paper）  
  lightweight speculative modelのdeep-layer hidden-state normをtarget LLMのcritical-token selectorとして使い、selective recomputationのselection overheadを削減する。full KV recomputation比でTTFT 2.17–3.95×短縮、throughput 2.7–5.2×向上を報告する。

### 2026-09-24

- **[KVShare: An LLM Service System with Efficient and Effective Multi-Tenant KV Cache Reuse](2025-2503.16525-kvshare.md)** — cited: **未確認**（user-specified; citation threshold不適用）  
  cross-request KV reuseをadaptive-length fragmentへ拡張し、attention-weighted deviationに基づくDual-Stage High Deviation (DHD)でprefill/decodeの両段階をselective repairする。現行v2を中心に、semantic-aware sharingを扱ったv1との大幅なversion差も明記する。

- **[A³: Attention-Aware Accurate KV Cache Fusion for Fast Large Language Model Serving](2025-2511.17560-a3.md)** — cited: **未確認**（user-specified; citation threshold不適用）  
  cached chunkのposition recoveryとquestion-to-document attentionによるtoken-level selective recomputationを組み合わせるquery-aware KV fusion。Qwen2.5-7B RULERで84.17を報告し、full prefillに対してTTFTを約2×改善する。

### 2026-09-21

- **[ProphetKV: User-Query-Driven Selective Recomputation for Efficient KV Cache Reuse in Retrieval-Augmented Generation](2026-2602.02579-prophetkv.md)** — cited: **未確認**（citation source: **未確認**; ICML 2026; user-specified）  
  CacheBlend型selective recomputationのtoken selectionをquery-aware化し、global saliencyではなくuser queryへのsemantic relevanceで再計算tokenを優先する。全layerのquery attentionをfusionするdual-stage pipelineにより、20% recomputationでfull-prefill accuracyの96–101%を維持し、RULERで既存SOTA比8.8–24.9%、LongBenchで18.6–50.9%のaccuracy改善を報告する。

### 2026-09-20

- **[Block-Attention for Efficient Prefilling](2024-2409.15355-block-attention.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ICLR 2025）  
  retrieved passageを独立blockとしてKV計算・再利用し、position re-encodingとblock-aware fine-tuningで品質低下を補う。32K inputでTTFT 98.7%、FLOPs 99.8%削減を報告する。

- **[CacheBlend: Fast Large Language Model Serving for RAG with Cached Knowledge Fusion](2024-2405.16444-cacheblend.md)** — cited: **未確認**（citation source: **未確認**; publication: EuroSys 2025）  
  prefix以外に配置されたdocument KVをそのまま連結すると失われるcross-document attentionを、重要tokenだけのselective recomputationで部分修復する。full prefill比でTTFT 2.2〜3.3×、throughput 2.8〜5×改善を報告する。

- **[EPIC: Efficient Position-Independent Caching for Serving Large Language Models](2024-2410.15332-epic.md)** — cited: **未確認**（citation source: **未確認**; venue fallback: ICML 2025）  
  chunk位置に依存しないPosition-Independent Cachingを定式化し、LegoLinkでdocument先頭に生じる不適切なattention sinkを低costに補正する。最大8×のTTFT改善と7×のthroughput向上を報告する。

## Scope notes

reuse手法は当面、次の3系統を同じcategory内で追跡する。

- **Inference-time repair** — cached KVを連結後、selective recomputation等でfull-context interactionを回復する手法。
- **Position-independent / modular reuse** — position re-encodingやlightweight linkingにより、任意位置でのcache reuseを可能にする手法。
- **Reuse-aware fine-tuning** — block-wise / concatenated KV reuseを前提にfine-tuningし、モデル自身をreuse-friendlyにする手法。
