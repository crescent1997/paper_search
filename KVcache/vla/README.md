# KV Cache — vla

Vision-Language-Action（VLA）モデルにおけるKV cache再利用・visual token削減・action生成の推論効率化に関する論文の要約と更新履歴。LLM向けの`reuse` / `compress` と重なる手法も、**連続観測フレーム間の時間的冗長性**や**ロボットのaction success / 制御周期**を主に評価する場合は本カテゴリで扱う。

## 更新履歴

### 2026-11-12

- **[SAFE-Pruner](2026-2605.29662-safe-pruner.md)** — cited: 4（mlsanity収録範囲、2026-10-20時点）; ECCV 2026; 自動選定・Historical  
  過去フレームの深層attention saliencyを利用して現在フレームの視覚tokenを選択する。Transformer KV tensor自体の再利用ではない。

- **[VLA-IAP](2026-2603.22991-vla-iap.md)** — cited: 1（mlsanity収録範囲、2026-10-19時点）; arXiv preprint; 自動選定・Historical  
  Semantic attentionとmotionの一致度、Sobel edgeを用いて物理操作に必要な視覚tokenを残す。KV tensor直接再利用ではない。


### 2026-11-11

- **[MEMI](2026-memi-reverse-engineering-memory.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; CoRL 2026 Late Breaking Results（プロジェクト記載、正式arXiv ID未確認）  
  行動予測誤差を通じて記憶すべき情報の教師信号を改善する。KV cacheの直接再利用ではない。

### 2026-11-10

- **★ [ReMEmbR](2024-2409.13682-remembr.md)** — cited: 77（mlsanity収録論文内、2026-11-04時点）; ICRA 2025  
  Captionと時空間メタデータを外部DBに保存し、LLMが反復検索する。RAVENとの比較対象で、Transformer KV cacheではない。

- **[HiMe](2026-2607.03449-hime.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  Planner・Sentry・Executorを階層化し、episodic memoryのAdd/Update/DeleteとPlanner呼出頻度を制御する。KV cache直接再利用ではない。


### 2026-11-09

- **[Chronos](2026-2606.30318-chronos.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  Selective SSMによる固定長recurrent stateで長時間の観測履歴を統合。KV cacheの直接再利用ではない。

- **[RAVEN](2026-2606.25206-raven.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  視覚埋め込みと位置・時刻を外部DBに保存し、長期ロボットQAとナビゲーションに利用。Transformer KV cacheではなく外部検索記憶。

- **[AURA](2026-2606.02775-aura.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  Action-error gateで固定容量fast-weight memoryの書き込みを制御。abstractのLIBERO評価と本文のsynthetic-only記述に不整合があり、実験結果の解釈には注意。


### 2026-11-08

- **[CondenseVLA](2026-2610.25927-condensevla.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; 採択venueは著者記載、公式未照合  
  過去フレームのキャッシュ済みVLM特徴をlearnable slotに圧縮し、diffusion action decoderへの履歴入力を削減する。Transformer KV cacheの直接再利用ではない。

- **[S²-VLA](2026-2606.27872-s2-vla.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; 採択venueは著者記載、公式未照合  
  GRUのbelief stateを使い、視覚・言語・行動特徴の融合を状態依存で制御する。長期タスクの状態追跡が中心で、KV cache直接再利用ではない。


### 2026-11-07

- **[IMPRINT](2026-2610.14406-imprint.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  視覚・action・proprioceptionを固定長のrecurrent latent memoryに圧縮。履歴再構成と進捗推定で事前学習後、VLAと共同微調整する。Transformer KVの直接再利用ではない。

### 2026-11-06

- **[SimpleMemVLA](2026-2609.05533-simplememvla.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  過去動画をnative-video contextとして入力し、action実行中に共有prefixをprefillして待ち時間を削減。KV prefix再利用を行うが専用の固定長memory bankは使わない。

### 2026-11-05

- **[ReMem-VLA](2026-2603.12942-remem-vla.md)** — cited: 未確認（ユーザー指定・引用数条件免除）; arXiv preprint  
  短期・長期のrecurrent queryと過去画像再構成の補助損失で視覚記憶を学習。KV cacheの直接再利用ではない。


### 2026-11-02

- **[★ LaWAM](2026-2606.15768-lawam.md)** — cited: 51（Semantic Scholar由来の表示値）; CoRL 2026（著者記載、公式未照合）  
  未来画像生成をlatent予測へ置換してaction推論を高速化。KV cacheの直接再利用ではない。

- **[HELM](2026-2604.18791-helm.md)** — cited: 7（mlsanity in-map）; arXiv preprint  
  Episodic memory検索と失敗予測・rollbackを組み合わせ、長時間タスク成功率を改善。KV再利用とは異なる。

- **[ST-Merge](2026-2606.29350-st-merge.md)** — cited: 1（mlsanity in-map）; arXiv preprint  
  Vision encoder内部の空間的visual token mergingで推論を効率化。VLA評価ではcross-frame KV reuseを実施していない。

### 2026-11-01

- **[EventVLA](2026-2606.20092-eventvla.md)** — cited: 26（mlsanity in-map）; CoRL 2026（著者記載、公式未照合）  
  将来必要な重要イベント画像を選んで保持する長期記憶。成功率を改善するが推論latencyは増加。

- **[μVLA](2026-2606.12497-muvla.md)** — cited: 11（mlsanity in-map）; arXiv preprint  
  Recurrent working memory tokenを学習して長期依存を保持。制御周波数とのtradeoffがある。

- **[AC²-VLA](2026-2601.19634-ac2-vla.md)** — cited: 6（mlsanity in-map）; arXiv preprint  
  Cognition feature再利用、visual token pruning、layer skippingを統合。layer KVそのものの再利用ではない。

### 2026-10-31

- **[TTF-VLA](2025-2508.19257-ttf-vla.md)** — cited: 12（mlsanity in-map）; AAAI 2026  
  前フレームのattentionと画像差分でvisual embeddingを選択的に再利用。KV cache全層の再利用とは異なる。

- **[VPWEM](2026-2603.04910-vpwem.md)** — cited: 6（mlsanity in-map）; IEEE RA-L 2026  
  Working memoryと固定容量episodic memoryを併用し、長期履歴の容量とlatencyを制御。

- **[EchoVLA](2025-2511.18112-echovla.md)** — cited: 15（mlsanity in-map）; ECCV 2026（共著者記載、公式未照合）  
  3D scene memoryとepisodic memoryを統合して長時間操作を支援。KV cache直接再利用ではない。

### 2026-10-30

- **[ChainVLA](2026-2608.02326-chainvla.md)** — cited: 未確認; 未確認  
  長期操作・記憶に関する論文。詳細は個別Markdownを参照。

- **[MemoryVLA++](2026-2606.09827-memoryvla-pp.md)** — cited: 未確認; 未確認  
  長期記憶設計に関する論文。詳細は個別Markdownを参照。

**未登録:** DepthCache（arXiv:2603.10469）はMarkdown作成済みだがGitHubへの登録がブロックされているため、一覧には未掲載。

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
- **Long-horizon memory** — working / episodic / semantic memoryの検索・更新・圧縮・忘却、部分観測下での長期タスク成功率とlatencyのtradeoffを扱う。KV cache直接再利用と区別する。
- **Streaming / runtime optimization** — incremental prefill、cacheの配置・転送、action chunkの非同期実行などでlatencyと制御周期を改善する。

同一論文は`compress` / `reuse` / `model` / `access`へ重複登録せず、必要に応じてカテゴリ間で相対リンクする。評価ではLIBERO・SIMPLER・実機でのtask success、CUDA/end-to-end latency、GPU memory、control frequency、追加学習要否を重視する。
