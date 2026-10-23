# VLA inference: KV cache reuse and visual-token efficiency

Vision-Language-Action (VLA)モデルにおけるKV cache・visual tokenの効率化研究を整理する。LLMと異なり、**連続観測フレーム間の時間的冗長性**と**action生成に必要な局所視覚情報**が中心的な論点になる。

| 論文 | 主分類 | 内容 |
|---|---|---|
| [VLA-Cache (NeurIPS 2025)](2025-2502.02175-vla-cache.md) | cross-frame KV reuse | 静止patchのKVを再利用し、task-relevant tokenを選択的に再計算。layer別attention entropyで再利用率を調整。 |
| [VLA-Pruner (arXiv 2025)](2025-2511.16449-vla-pruner.md) | action-aware visual-token pruning | prefillのsemantic attentionと過去action attentionを併用して重要tokenを保持。KV自体の時間的再利用ではない。 |

## 今後の分類軸
- `cross-frame reuse`: visual KVの時系列再利用、位置合わせ、cache更新。
- `action-aware pruning`: 行動生成に必要なtokenを残すためのattentionベースの圧縮。
- `architecture/model`: VLAのaction head、diffusion/flow matching、temporal encoderに応じたcache構造。
- `runtime/system`: end-to-end制御周期、cacheの転送・保持、batching・非同期実行。

同じ論文を`reuse`や`compress`に重複登録せず、VLA固有の評価（LIBERO、SIMPLER、実機成功率、control frequency）を優先する。