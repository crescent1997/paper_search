---
title: "LogicInception: Discourse-Level Indirect Prompt Injection via Causal Chain Construction"
summary: "外部tool outputに命令文を置かず、複数の宣言的断片からLLM自身に偽の因果的依存関係を推論させる間接prompt injection。AgentDojo/InjecAgent計1,139シナリオでASR 42.9/48.8%、断片単位stealth約88%。GCG/Imprompterの最適化suffixとは異なるreasoning経由の信頼境界越えを分析する。"
authors_affiliations: "Mingshuai Liu, Yijie Peng, Chen Dongxian, Mingfeng Li, Xin Li — School of Information and Cyber Security, People's Public Security University of China; Xin Li also: Public Security Big Data Strategy Research Center, Beijing, China"
published: "2026-10"
publication_status: "EMNLP 2026 Main, pages 41686–41702 (ACL Anthology: 2026.emnlp-main.1831); published October 2026"
topics: ["indirect prompt injection", "reasoning-induced instruction conflict", "tool-output trust boundary", "discourse-level attack", "causal chain", "black-box attack", "authority spoofing"]
cited: null
citation_source: "Not independently verified as of 2026-11-02; accepted EMNLP 2026 Main (official ACL Anthology) used as venue fallback"
url: "https://aclanthology.org/2026.emnlp-main.1831/"
code: "https://github.com/XLShawn1110/LogicInception"
last_checked: "2026-11-02"
---

# 概要と位置づけ
**区分: Attack。** 従来のindirect prompt injection (IPI)は、tool outputやretrieved documentに「別の命令」を挿入し、モデルが上位のuser/system instructionより低信頼テキストを優先する現象を利用する。LogicInceptionはその前提を変える。攻撃断片には直接の命令文を含めず、業務上の状態・条件・依存関係のように読める宣言文を配置し、モデルが正規タスクの遂行に必要な前提として無許可のtool actionを**自ら推論**するよう誘導する。これはInstruction Hierarchyのrole-label spoofingそのものではなく、低信頼な「事実らしい記述」が高信頼の行動根拠へ昇格する**reasoning-mediated trust-boundary crossing**である。

一次資料: [ACL Anthology](https://aclanthology.org/2026.emnlp-main.1831/) / [PDF](https://aclanthology.org/2026.emnlp-main.1831.pdf)。著者・所属・発表年・会議録番号はPDF冒頭と公式書誌を照合。確認できるarXiv IDはないため、ファイル名はACL Anthology IDを採用した。

# Problem / Threat Model
攻撃者はagentが参照する**少なくとも1つのfree-text外部データ源**（メール、RAG結果、サービス通知、tool output等）へ文字列を挿入できる。公開APIから対象tool名・引数形式は知るが、**target modelのweights、gradient、logit、query accessを持たず、targetへの反復問い合わせも行わない**。system prompt、正規user instruction、agent runtimeの直接変更はできない。目的は正規ユーザーが許可していない特定のtool callを実行させること。主張する攻撃の成立条件は、外部記述の解釈と行動決定が同じモデル内で結びついていることである。

**Attack primitive:** tool outputの宣言的な偽依存関係（fabricated causal dependency）。**配置:** in-context/tool-result、複数sourceへ分散可能。**GCGとの相違:** ユーザー末尾のadversarial suffixやwhite-box token gradient最適化は用いない。**black-box:** 攻撃生成器としてGPT-4oを1回呼び出すが、targetへは問い合わせない。**query/compute cost:** 約120生成tokens、約5.6秒、生成API call 1回（論文§5.2）。**transfer:** 複数target familyで同一手法を評価するが、厳密な同一payloadのcross-model転移成功率を測った実験とは区別する。

# Method / Algorithm
1. **Register-Constrained Generation (§3.2):** 各外部sourceの文体に合わせた宣言文を作る。論文は命令形、security-sensitive語、明示的なtool名/API呼び出し構文を排し、パラメータを通常の記録値として見せる制約を設定。500断片のspeech-act分析ではdirective speech actが0%（比較baselineは85–100%）。これは検出器の出力と人手確認を伴う論文内の分析であり、あらゆる文体に普遍的な保証ではない。
2. **Causal Chain Construction (§3.3):** ユーザーtaskを`t`、不正なtarget actionを`a*`、断片を`F={f1,...,fn}`とする。agent reasoning `R(t,F)` が `a*` を選ぶような依存関係を組み立てる。論文の設計条件は、適切な真部分集合`S⊂F`では同じactionが導かれないという**non-decomposability**。ここでの「causal」は統計的因果効果ではなく**捏造された物語上の前提関係**を指す。
3. **Cross-Source Fragment Placement (§3.4):** 意味的に近いtool responseへ通知・記録の形で追加。AgentDojoの74.4%のシナリオは挿入点が1か所しかないため、その場合は複数断片を同一出力内で結合する。cross-source配置は成功の必須条件ではない。
4. **実行:** targetが通常のtask reasoningとtool selectionを行うだけで、攻撃者が要求した行動が「必要な準備」に見える。防御上の論点は、指示の有無だけでなく、**行動の根拠が信頼済みuser goalから来ているか**の検証である。

# Experiment Setup
- **AgentDojo:** 629 scenarios、workspace/slack/travel/bankingの4 suite、97 tools。
- **InjecAgent:** 510 scenarios、17 attack categories。計1,139 scenarios。
- **7 target models:** GPT-5.4, Claude-4.5-sonnet, Gemini-3-flash, DeepSeek-V3.2, Qwen-3.5plus, GLM-5, Kimi-2.5。target temperature 0、最大15 interaction steps。generator GPT-4o temperature 0.7。
- **Attack baselines:** Direct Injection、TopicAttack、Fuzzing、Imprompter（white-box optimized suffix）。
- **5 defenses:** Delimiting、Repeat-User、Instructional Prevention、PI-Detector（GPT-4o-mini）、MELON（masked re-execution）。**StruQ / SecAlign / Meta SecAlign / CaMeL / ISEへの直接評価はない**。したがってこれらの防御に成功したとは書けない。
- **Metrics:** ASR=不正tool actionと引数が一致した割合。Stealth=3検出器（Llama Guard 3、DeBERTa PI、GPT-4o-mini）の非検出率。`C-ASR = ASR × mean(stealth)`は独立性の近似に依存する集約指標であり、実測の同時成功確率と完全に同義ではない。UTI=正規task完了率。

# Main Results
| Benchmark (7-model平均) | Direct Injection ASR | TopicAttack ASR | Imprompter ASR | LogicInception ASR | LogicInception C-ASR |
|---|---:|---:|---:|---:|---:|
| AgentDojo | 9.8% | 26.1% | 29.2% | **42.9%** | **37.8%** |
| InjecAgent | 11.9% | 28.4% | 33.6% | **48.8%** | **42.9%** |

AgentDojoでLogicInceptionの平均stealthは約88.2%、InjecAgentでは約87.9%。AgentDojoの比較baselineのC-ASRは最大6.8%、InjecAgentでは最大7.4%。論文はMcNemar検定（Bonferroni補正）で差を報告（p<0.001）。Claude-4.5-sonnetではDirect InjectionがAgentDojo **1.2%**なのに対しLogicInceptionは**21.8%**、18.2倍。DeepSeek-V3.2は61.7%/67.3%（AgentDojo/InjecAgent）。**ASRが高いことと正規utilityが高いことは同じではない**。平均AgentDojo UTIはclean 69.1%から65.1%へ4.0ポイント低下。

# Additional Evaluations / Robustness
- **Ablation（AgentDojo）:** GPT-5.4でfull ASR 36.9%→causal-chain除去19.4%、Qwen-3.5plusで56.3%→29.4%。register制約除去ではstealth約90%→約38%。cross-source placement除去では36.9→26.0%、56.3→39.7%。断片を1つへ統合するとGPT-5.4のASRは21.1%。したがって因果連鎖と文体制約は異なる役割を担う。
- **Defense:** prompt-level 3防御はDirect Injectionを31–72%相対削減する一方、LogicInceptionの相対削減は最大7%。PI-Detectorの相対削減は約12%。MELONではGPT-5.4の36.9→26.3%、Claudeの21.8→14.6%。
- **Adaptive detector:** DeBERTaを攻撃400件+benign400件で再学習するとstealth 79.0→31.3%、C-ASR 37.8→13.4%（FPR 14.2%）。「検知不能」ではなく**検出器の再学習で一定程度対抗可能**。
- **Self-verification:** tool execution前のuser-goal grounding確認でGPT-5.4 ASR 36.9→24.8%、MELON併用19.1%。一方clean utilityは単独で4.8ポイント、併用で6.3ポイント低下。
- **Capability relation:** clean utilityとLI/DI倍率にSpearman ρ=0.79（p=0.036, n=7）。モデル間の**相関**であってreasoning能力が攻撃を因果的に増やすと証明したものではない。

# Limitations / Unmeasured Conditions
free-text tool outputとtool schemaの事前知識が必要。structured-only JSON API、production deployment、非英語registerへの転移は未検証。detectorごとの結果やadaptive再学習後の残存ASRは設定依存。単一fragmentの平均長がbaselineより長く、length-based検出も可能。モデルAPI snapshotの変更で絶対値は変わりうる。targetがsystem/developerの役割境界を明示的に破る場面と、外部の偽事実を信じて不正tool actionを選ぶ場面を混同しない。

# LLM Model Security Taxonomy / 研究上の示唆
**Attack > Indirect prompt injection > Reasoning-mediated authority laundering / fabricated prerequisites.** GCG系の「攻撃命令を従わせる」問題を越え、モデルが**攻撃命令を見なくても**未承認actionを正当化できる点が新しい。モデル学習では、正しいtool actionと不正な前提依存を識別する反実仮想データ、tool-call前のuser-authorized provenance評価、事実として使ってよい情報と権限を付与してよい情報の分離が課題。runtime側では明示的なtool権限・egress制限が補完的に必要。

# 関連研究（repository相対リンク）
- [GCG — white-box suffix optimization](../Attack/2023-2307.15043-gcg.md): 対照的な攻撃primitive。GCGの成功率をLogicInceptionへ外挿できない。
- [ChatInject — dialogue role confusion](../Attack/2025-2509.22830-chatinject.md): 低信頼テキストに偽会話指示を混入する系列。
- [SecAlign — preference optimization](../Defense/2024-2410.05451-secalign.md): 比較対象のmodel-level defenseだが**直接評価なし**。
- [StruQ — structured prompt/data boundary](../Defense/2024-2402.06363-struq.md): 直接評価なし。
- [CaMeL — explicit capability/provenance](../Defense/2025-2503.18813-defeating-prompt-injections-by-design-camel.md): 理論的に重要な対照、直接評価なし。

# 出典照合メモ
公式ACL Anthology PDF §§3–6、Tables 1–3、defense/adaptive評価およびLimitationsを確認。arXiv IDを未確認のため付与しない。引用数は確証のある集計値を取得できず、公式EMNLP 2026 Main採択をvenue fallbackとした。