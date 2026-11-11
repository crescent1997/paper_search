---
title: "MEMI: Reverse-Engineering What Robots Should Remember"
summary: "過去の観測から何を記憶するかを、全軌跡を見られるannotatorとオンラインpolicyの両promptを反復改善して学習する。sufficiency/predictability/necessityを評価し、RoboMMEでplanner 99.4%、end-to-end 97.0%を報告。KV cacheの直接最適化ではない。"
authors_affiliations: "Sooyoung Ryu, Yinpei Dai, Yilun Du, Chelsea Finn, Yuejiang Liu; 所属は一次資料で未確認"
published: "2026（公開日未確認）"
publication_status: "Late Breaking Results in Agentic Robotics, CoRL 2026（公式プロジェクト記載。正式論文採択とは区別）"
topics: [VLA, long-term-memory, memory-supervision, prompt-optimization, episodic-memory, RoboMME]
cited: null
citation_source: "未確認（ユーザー指定のため閾値免除）"
url: "https://memi-robot.github.io/"
code: "不明（公式ページにCodeリンクがあるが公開先未検証）"
last_checked: "2026-11-11"
---

# MEMI — Reverse-Engineering What Robots Should Remember

## 1. 概要・問題設定
長期ロボット操作では現在の画像だけでは行動が決まらない。従来の記憶方式は人間・LLMが選んだkeyframeやsummaryを教師として与えるが、デモに含まれるのは行動であり、その行動を決めるための**正しい記憶**は観測できない。MEMIは記憶教師を未知変数として扱い、デモを説明するために何を保持すべきかを逆問題として推定する。オンラインpolicyの記憶はfield–value pair（テキスト値、必要に応じ画像keyframe）で表現する。

## 2. 先行研究との差
[MEM](https://arxiv.org/abs/2603.03596)は短期video memoryと長期language memoryを組み合わせ、VLAのモデル構造と学習を設計する。一方MEMIは**どの記憶フィールドを作るか、その内容をオンラインで再現できるか**を行動誤差から改善する。固定された記憶表現のretrievalやrecurrent latent stateではなく、**記憶教師の生成規則そのものを最適化**する。RAVEN/ReMEmbRの外部DB検索とも異なり、LLM/VLM policyが現在の記憶を更新して行動する。

## 3. 手法
### 3.1 記憶の逆推定
デモ集合 E はtask qと観測・行動対 (o_t,a_t) を含む。annotator α_φ は**未来を含む軌跡全体**から記憶教師 m̃_1:T を生成する。policy π_θ は (q,o_t,m̃_{t−1}) を受け、記憶 m_t と行動 â_t を予測する。学習中はteacher forcingで正解側の記憶を入力し、実行時には自身が前stepで生成した m_{t−1} を入力する。この差は誤差蓄積の潜在要因である。

### 3.2 三つの原則
- **Sufficiency**: 予測行動 â_t とデモ行動 a_t の誤差を減らし、必要な情報の欠落を検出する。
- **Predictability**: policyが未来情報なしでannotatorの記憶教師 m̃_t を再現できるか測る。hindsight leakageを防ぐ。
- **Necessity**: policyが使用したと申告しないフィールドを除外し、冗長な記憶を抑える。使用申告は真の因果的重要性の近似である。

三目的のフィードバックを使いTextualSGDがannotator prompt φとpolicy prompt θを同時に更新する。各反復では新しいデモminibatchを使用し、annotatorによる再注釈→policy予測→失敗評価→prompt修正を行う。**訓練は記録済みデモ上のoffline処理**であり、annotatorは実行時には不要。ただし推論policyにはreasoning modelの呼出しが残る。

## 4. Experiment Setup
- **RoboMME**: 記憶依存の16タスク。StopCube、RouteStick、VideoPlaceOrderなどを含む。
- **Plan**: テキスト観測（semantic segmentation由来の物体位置）から言語subgoalと画面上の位置を出力し、benchmarkの**ground-truth low-level controller**が実行。したがって純粋なend-to-end manipulationとは区別。
- **Act**: front/wrist画像とrobot stateからend-effector waypointとgripper指令を直接生成。記憶にはkeyframeも保存可能。
- **Baselines**: FullHist（全履歴）、Init（初期の固定記憶仕様）、MEMI（共同最適化）。複数reasoning modelで検証。
- **追加学習**: weight fine-tuningではなくoffline prompt optimizationとtrajectory annotationが必要。反復回数・API総費用の詳細な総括値は公式ページだけでは確認できない。

## 5. Main Results
| 条件 | Init | MEMI | 備考 |
|---|---:|---:|---|
| Luna-Plan | 66.6% | 85.4% | low-level controllerはoracle |
| Haiku-Plan | 73.9% | 90.8% | 同上 |
| Fable-Plan | 88.6% | 97.1% | 同上 |
| Astra-Plan | 92.4% | **99.4%** | 同上 |
| Astra-Act | 88.3% | **97.0%** | 直接waypoint予測 |

FullHist-Astra-Actは85.3%。公式ページではAstra-Actの既報最高92.1%との比較も示すが、実験条件を揃えた再現比較かは未確認。PlanとActの数値を同じpolicy能力として比較しない。

## 6. 主要ablation
- StopCube: sufficiency feedbackを除外すると成功率94%→36%。過去位置を記憶へ追加できず、ボタン押下のタイミングを誤る。
- RouteStick: predictability feedbackを除外すると84%→18%。オンラインで再現できない記憶を使う問題。
- VideoPlaceOrder: necessity feedbackを除外すると成功率はほぼ同じだが、不要フィールドが残り**episode当たり推論コストが24%増加**。
- StopCubeの反復事例では、位置履歴フィールドの追加で成功率34%→84%、後のpolicy prompt改善で94%。記憶の内容と利用規則の両方が重要。

## 7. Limitations / 効率性
1. **KV cache最適化ではない**: TransformerのK/V再利用、eviction、prefill高速化は実装しない。memory fieldを減らせばpromptコストは減りうるが、KV cacheの量・TTFTを測ったものではない。
2. **latency**: necessity ablationのepisode推論コスト24%以外に、per-step latency、throughput、GPU peak memory、control Hz、end-to-end wall timeの系統比較は確認できない。
3. **訓練コスト**: annotatorが全軌跡を読むoffline反復最適化を必要とし、推論時のmemory annotationは不要でも開発時のAPI/推論費用は発生する。
4. **評価範囲**: RoboMME 16タスクが中心で、現実の数十分～数時間の連続実機運用や、メモリ容量上限・忘却による頑健性は未確認。
5. **予測可能性の限界**: teacher forcing学習とself-generated memoryの実行時分布差が残る。necessityのフィールド使用申告は自己報告で、因果的ablationではない。
6. **公開情報**: 正確なarXiv IDは公式ページで確認できず、公式project postとして扱う。所属、コードURL、正式論文PDFの一次照合は保留。

## 8. Taxonomy / 関連研究
- Primary: `KVcache/vla/` / memory learning, supervision, write/update, long-horizon manipulation。
- [MEM](https://arxiv.org/abs/2603.03596): video短期＋language長期のマルチスケール記憶。MEMIは記憶教師の設計を逆推定する。
- [MemoryVLA](2025-2508.19236-memoryvla.md): perceptual-cognitive memory bankを用いるVLA。
- [RAVEN](2026-2606.25206-raven.md): 外部visual embedding検索。
- [ReMEmbR](2024-2409.13682-remembr.md): captionベース外部episodic retrieval。
- [HiMe](2026-2607.03449-hime.md): 明示的Add/Update/Deleteとplanner/sentry/executor分離。

## 9. Sources
- [MEMI公式プロジェクト全文（Method, Results, Ablations, Citation）](https://memi-robot.github.io/)
- [RoboMME](https://arxiv.org/abs/2603.04639)
- [MEM](https://arxiv.org/abs/2603.03596)