# KV Cache — vla

Vision-Language-Action（VLA）モデルにおけるKV cache再利用・visual token削減・action生成の推論効率化に関する論文の要約と更新履歴。LLM向けの`reuse` / `compress` と重なる手法も、**連続観測フレーム間の時間的冗長性**や**ロボットのaction success / 制御周期**を主に評価する場合は本カテゴリで扱う。

## 更新履歴

### 2026-10-29

- **[Gated VLA-Cache](2026-2608.10824-gated-vla-cache.md)** — cited: 1（mlsanity in-map）; IROS 2026  
  Action tokenの予測確信度を使ってKV再利用と全KV再計算を切り替えるtraining-free手法。長時間制御の信頼性を改善する。

- **[The Gate, Not the Cache](2026-2608.00391-the-gate-not-the-cache.md)** — cited: 2（mlsanity in-map）; arXiv preprint  
  Skip済みforward由来のgateが起こす自己強化的な失敗を分析し、action実行中のdense refreshを提案する。

- **[ActionCache](2026-2607.06370-actioncache.md)** — cited: 2（mlsanity in-map）; arXiv preprint  
  Flow-matching VLAで過去の中間actionを再利用してdenoisingを高速化する。KV cacheの直接再利用とは異なる。

### 2026-10-28

- **[TempoFit](2026-2603.07647-tempofit.md)** — cited: 5（mlsanity in-map）; arXiv 2026 preprint  
  過去フレームの中間層KVを長期記憶として保持し、類似度検索と時間バイアスを用いて現在の推論へ統合する。追加学習なしで長期タスク成功率を改善する。

- **[NativeMEM](2026-2607.06678-nativemem.md)** — cited: 9（mlsanity in-map）; CoRL 2026  
  Action-supervised memory tokenizerで各frame-viewを1 tokenに圧縮し、長期履歴を効率的に利用する。KV cacheの直接再利用ではなく、追加学習が必要。

- **[OxyGen](2026-2603.14371-oxygen.md)** — cited: 4（Scholar Feed収録範囲内）; NeurIPS 2026  
  同一観測からのactionと言語生成でVLM prefill/KVを共有し、continuous batchingで並列実行を効率化する。フレーム間KV再利用とは異なる。

### 2026-10-27

- **★ [MemoryVLA](2025-2508.19236-memoryvla.md)** — cited: 199（mlsanity in-map、ユーザー指定）; ICLR 2026  
  Working MemoryとPerceptual-Cognitive Memory Bankを用いて過去の視覚・意味情報を検索・統合し、長期操作を支援する。KV cacheの直接再利用ではない。

- **★ [MEM](2026-2603.03596-mem.md)** — cited: 58（Scholar Feed収録範囲内）; arXiv 2026 preprint  
  短期動画記憶と長期言語記憶を組み合わせ、時間方向attentionで過去フレームを集約する。下流VLMへの履歴token入力を抑える。

- **[MemBodied](2026-2609.28256-membodied.md)** — cited: 1（mlsanity in-map）; arXiv 2026 preprint  
  Gated Delta Ruleによる固定容量の連想記憶行列で過去のactionと観測結果を保持し、長時間タスクでの記憶容量と推論latencyを抑える。KV cacheそのものの再利用ではない。

### 2026-10-25

- **★ [EfficientVLA](2025-2506.10100-efficientvla.md)** — cited: 94（mlsanity in-map、Google Scholar総引用数ではない）; arXiv 2025 preprint（採択venue未確認）  
  LLM layerとvisual tokenを削減し、diffusion action headのdenoising step間でattention/MLP中間特徴を再利用する。連続フレーム間のKV再利用とは異なる時間軸で推論を高速化する。

- **[LAC](2026-2602.00686-lac.md)** — cited: 2（mlsanity in-map）; arXiv 2026 preprint（採択venue未確認）  
  Optical flow・attention alignment・task lossを利用してvisual KVの再計算tokenと再利用率を学習する。VLA-Cacheのtraining-free selectionと異なり軽量moduleの追加学習が必要。

- **[Reflex](2026-2607.14695-reflex.md)** — cited: 3（mlsanity in-map）; ICML 2026 Main  
  Flow matchingのtimestep依存性に合わせてstatic prefix・sliding visual history・dynamic action suffixを分離し、正しいKV再利用とincremental prefillを実現する。非同期制御によるaction実行の重畳も扱う。

### 2026-10-23

- **[VLA-Cache](2025-2502.02175-vla-cache.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; NeurIPS 2025  
  連続観測フレームで変化の少ないvisual tokenのKVを再利用し、task-relevant tokenだけを再計算する。layer別attention entropyで再利用率を調整するtraining-free手法。

- **[VLA-Pruner](2025-2511.16449-vla-pruner.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv 2025 preprint（採択venue未確認）  
  Prefill時のsemantic attentionと過去のaction attentionを組み合わせ、ロボット操作に必要なvisual tokenを保持する。KVの時間的再利用ではなくaction-aware pruningを行う。

## Scope notes

- **Cross-frame KV reuse** — 連続観測フレームの変化しないvisual tokenを再利用し、変化したtokenやtask-relevant tokenを更新する。
- **Action-aware visual token pruning** — 言語理解だけでなく操作成功率への寄与を考慮してvisual tokenを削減する。
- **Diffusion / flow-matching feature reuse** — denoising timestep間のattention・MLP中間特徴や、timestepに依存しないKVを安全に再利用する。
- **Streaming / runtime optimization** — incremental prefill、cacheの配置・転送、action chunkの非同期実行などでlatencyと制御周期を改善する。

同一論文は`compress` / `reuse` / `model` / `access`へ重複登録せず、必要に応じてカテゴリ間で相対リンクする。評価ではLIBERO・SIMPLER・実機でのtask success、CUDA/end-to-end latency、GPU memory、control frequency、追加学習要否を重視する。
