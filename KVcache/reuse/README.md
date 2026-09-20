# KV Cache — reuse

異なるrequest / document / prompt module間でprecomputed KV cacheを再利用し、prefill計算とTTFTを削減する論文の要約と更新履歴。推論時のKV再配置・部分再計算だけでなく、**KV再利用を前提にfine-tuningしてモデルを適応させる手法**も対象とする。

## 更新履歴

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
