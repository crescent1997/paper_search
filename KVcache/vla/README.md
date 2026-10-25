# KV Cache — vla

Vision-Language-Action（VLA）モデルにおけるKV cache再利用・visual token削減・action生成の推論効率化に関する論文の要約と更新履歴。LLM向けの`reuse` / `compress` と重なる手法も、**連続観測フレーム間の時間的冗長性**や**ロボットのaction success / 制御周期**を主に評価する場合は本カテゴリで扱う。

## 更新履歴

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
