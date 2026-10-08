---
title: "A Systematic Study of Cross-Layer KV Sharing for Efficient LLM Inference"
summary: "LCKV・CLA・YOCO等を統一したcross-layer KV sharing topologyで比較。2倍程度の削減では多くの構成が競争力を保つが、強い削減ではupper-layer KV sharingが品質面で有利になり得る。"
authors_affiliations: "You Wu, Haoyi Wu, Kewei Tu (ShanghaiTech University)"
published: "2025-04"
publication_status: "NAACL 2025 Short Papers"
topics: ["KV Cache", "Model", "Cross-Layer Sharing", "Topology", "Benchmark"]
cited: 17
citation_source: "Semantic Scholar-related aggregation reported in prior selection; direct verification pending; NAACL official venue verified"
url: "https://aclanthology.org/2025.naacl-short.34/"
code: "https://github.com/whyNLP/LCKV"
last_checked: "2026-10-08"
---

# A Systematic Study of Cross-Layer KV Sharing for Efficient LLM Inference

## 概要
Cross-layer KV sharingにはLCKV、CLA、YOCOなど複数方式があるが、どのlayerがKVを作り、どのlayerがそのKVを参照するかという**sharing topology**で統一できる。本研究はそのconfigurationを同一frameworkで評価する。

## 問題設定
cross-layer sharingはlayer数に比例するKV memoryを減らせる一方、品質・training cost・prefill latency・throughputがtopologyに依存する。個別論文の異なる設定だけでは最適構成を判断しにくい。

## 手法・topology
各layerのqueryとKV生成layerの対応を定義し、LCKVのupper-layer sharing、CLAの近接bottom-layer sharing、YOCO系のglobal sharing、および新しいconfigurationを比較する。主な観点は、KV生成layerの位置と共有groupの形状。

## Experiment Setup
language modeling、downstream tasks、generation throughputを統一実装で比較する。KV cacheを2×削減する設定と、より強い削減設定を分析し、prompt length依存性も検証する。

## Main Results
**2× KV reduction**では多くの構成がstandard Transformerより高throughputを達成しつつ競争力あるqualityを維持する。さらに強い削減ではbottom-layer KVだけを使う方式のquality低下が大きくなり、upper-layer KVを全layer queryから利用する構成が優れる。ただしtraining costとprefill latencyが増える。

## Ablation / Analysis
短promptでは各configurationのthroughput改善が大きい一方、長promptではtop-layer KVを計算する構成のthroughputが大きく低下する。したがってdecode時memory効率だけではなく、prefill dependencyを含めて設計する必要がある。

## Limitations
- 結果は比較したmodel scaleとtraining setupに依存。
- upper-layer sharingは強いcompressionで有利でも長promptのprefill penaltyがある。
- pretrained modelへの無変更導入を保証しない。
- 実運用ではbatch size・context length・GPU memoryの分布で最適構成が変わる。

## Taxonomy・関連研究
**model / systematic cross-layer KV sharing / topology study**。

- [LCKV](2024-lckv.md) — upper-layer condensed sharing。
- [Cross-Layer Attention](2024-2405.12981-cross-layer-attention.md) — grouped bottom-layer sharing。
- [YOCO](2024-2405.05254-yoco.md) — decoder-decoder sharing。
- [KVSharer](2024-2410.18517-kvsharer.md) — pretrained modelへのsharing map導入。
