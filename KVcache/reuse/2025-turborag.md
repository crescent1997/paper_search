---
title: "TurboRAG: Accelerating Retrieval-Augmented Generation with Precomputed KV Caches for Chunked Text"
summary: "retrieved chunkのKVをoffline生成し、independent attentionとreordered RoPEでonline結合するRAG向けKV reuse。標準RAG比でTTFT平均8.6倍、最大9.4倍短縮を報告する。"
authors_affiliations: "Songshuo Lu, Hua Wang, Yutian Rong, Zhi Chen, Yaohua Tang (affiliations: see ACL Anthology)"
published: "2025-11"
publication_status: "EMNLP 2025 Main"
topics: ["KV Cache", "Reuse", "RAG", "Offline Prefill", "Independent Attention", "RoPE"]
cited: 7
citation_source: "Secondary citation source reported in prior selection; exact provider unverified; venue fallback: ACL Anthology EMNLP 2025 Main"
url: "https://aclanthology.org/2025.emnlp-main.334/"
code: null
last_checked: "2026-10-08"
---

# TurboRAG

## 概要・問題設定
RAGでは検索したchunkを毎回prefillすることがTTFTの大きな負担になる。TurboRAGはchunkのKVをofflineで計算・保存し、onlineではretrieved chunkに対応するcacheを連結して再利用するhybrid offline–online方式。

## 既存法の問題
通常のprefix cachingでは、検索順序が変わったdocumentのKVは同じprefixとして再利用できない。[RAGCache](2024-2404.12457-ragcache.md)もprefix依存性を保持する。独立に生成したchunk KVをそのまま連結するとattention contextとRoPE positionが通常のsequential prefillと異なる。

## 手法
TurboRAGは**independent-attention**によってchunkごとのattentionを独立化し、offline precomputation可能な形式を採る。さらに**reordered-RoPE**で結合時のpositionを扱う。onlineでは検索chunkのKVを読み込み、指定順にstitchしてquestion側を処理する。

```text
offline: chunk → independent attention → cached KV
online: retrieval → cache load → reordered RoPE / stitch → answer
```

注意点として、著者の公式abstractは**model architectureやinference systemの変更を必要としない**と明記している。先行選定時の「fine-tuningが必要」という説明は公式abstractでは裏付けられないため、本要約では前提にしない。

## Experiment Setup / Main Results
複数RAG benchmarksでstandard RAGとのqualityとTTFTを比較。論文報告ではTTFTが**平均8.6×、最大9.4×**改善し、answer qualityはstandard RAGと同程度を維持する。

## Ablation・trade-off
主要な検証観点は独立attentionの品質影響、position handling、offline KV作成とonline cache loadingの切り分け。prefill計算をonlineから除く代わりに、KV保存容量・転送量・cache hit率がend-to-endの効率を左右する。

## Limitations
- independent attentionは通常のfull-context attentionと同一ではない。
- cached chunk数に比例するstorage costとload bandwidthが必要。
- corpus/model変更時にoffline KV再生成が必要。
- TTFT改善はoffline costを除いたonline serving条件に依存する。

## Taxonomy・関連研究
**reuse / offline chunk KV / position correction / RAG serving**。

- [RAGCache](2024-2404.12457-ragcache.md) — prefix-sensitive knowledge caching。
- [CacheBlend](2024-2405.16444-cacheblend.md) — runtime selective recomputationでcross-chunk interactionを修復。
- [Cache-Craft](2025-2502.15734-cache-craft.md) — chunk cacheのrepairとmanagement。
- [KVLink](2025-2502.16002-kvlink.md) — learned link tokensによる独立document KVの結合。
