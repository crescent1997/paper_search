---
title: "Many-Tier Instruction Hierarchy in LLM Agents"
summary: "固定されたsystem/user/tool role hierarchyを超え、最大12段階のarbitrary privilege tierを持つ853 agent tasksでLLMのpriority reasoningを評価するManyIH-Benchを提案し、2-tierで高性能な最新モデルもmany-tierでは大幅に失敗することを示す。"
authors_affiliations: "Authors / affiliations as reported in the paper"
published: "2026"
publication_status: "arXiv preprint"
topics: ["LLM Security", "Instruction Hierarchy", "Privilege Separation", "Benchmark", "LLM Agents", "Many-Tier Priority"]
cited: null
citation_source: "未確認"
url: "https://arxiv.org/abs/2609.11589"
code: "https://github.com/amazon-science/manyih"
last_checked: "2026-09-16"
---

# Many-Tier Instruction Hierarchy in LLM Agents

> system > userのような固定role hierarchyを「privilege reasoning」の特殊ケースとして捉え直し、instructionごとに明示的なpriority levelを与えた最大12-tierのagent scenarioを評価する。最新LLMは2-tierではほぼ完全に競合を解決できてもtier数が増えると急激に崩れ、role名ではなく抽象的なpriority orderingを一般化する能力が未成熟であることを示す。

## 概要

従来のInstruction Hierarchy研究では、system / developer / user / toolといった少数のroleに固定されたpriorityを扱うことが多い。しかし実システムではorganization policy、application developer、workspace administrator、user、sub-agent、tool、remote contentなど、より多段のauthorityが存在しうる。

Many-Tier Instruction Hierarchy (ManyIH) はinstruction hierarchyをrole semanticsから切り離し、各instructionに独立したprivilege tierを割り当てる。この能力を測る **ManyIH-Bench** は **853 tasks、46 agent scenarios、最大12 tiers** を含む。

## 問題設定

複数instruction `I_1 ... I_n` にpriority `p_1 ... p_n` が与えられ、互いにconflictする場合、モデルは最も高priorityで適用可能なinstructionを選択する必要がある。

重要なのは単純なsystem-vs-user binary classificationではなく、

```text
Tier 1 > Tier 2 > Tier 3 > ... > Tier 12
```

というorderingをscenarioごとに解釈し、複数のconflictを解決することである。

privilege labelとsemantic roleを分離するため、「systemだから強い」というchat-training由来のshortcutでは解けない。

## 先行研究からの改善点

[The Instruction Hierarchy](../Defense/2024-2404.13208-instruction-hierarchy.md) はprivileged instruction trainingを導入し、[IHEval](2025-2502.08745-iheval.md) はsystem / user / conversation / toolという固定された4-level hierarchyを評価した。

ManyIHは次の問いへ進む。

> モデルは既知roleのpriorityをmemorizeしているだけなのか、それとも任意のpriority relationを抽象的にreasoningできるのか？

このためrole identityとprivilege valueを分離し、tier数を2から6、8、12へ増加させる。

## ManyIH-Bench

benchmarkは46のagent-oriented scenariosから853 tasksを構成する。scenarioには複数sourceからのinstruction / dataが入り、異なるprivilege level間にconflictが設定される。

評価ではtier数だけでなくpriority表現も変える。例えばordinal representationとscalar-like representationを用いることで、同じorderingでも表記方法が変わった際にgeneralizeできるかを見る。

これにより、モデルが単なる「Tier 1という文字列は強い」といったsurface patternを学んでいるか、ordering relationそのものを利用しているかを診断できる。

## Experiment Setup

GPT、Gemini、Claude等の最新proprietary modelを含む複数LLMを評価する。主metricはconflicting instructionsの中からhighest-privilege policyを満たしてtaskを完了できた割合である。

2-tier settingをbaselineとしてmany-tierへ段階的に拡張し、reasoning effortを変更可能なモデルではinference-time reasoning量による改善も調べる。

## Main Results

2-tier hierarchyでは複数の最新modelが **99%以上** のperformanceを示し、一見instruction hierarchy問題がほぼ解決したように見える。

しかしmany-tierへ拡張すると性能は大幅に低下する。代表的なoverall scoreは、

- **Gemini 3.1 Pro: 42.7%**
- **GPT-5.4: 39.4%**

にとどまる。

さらにtier数を **6 → 8 → 12** と増やすにつれて、ほぼ全モデルでaccuracyが単調に低下する。したがってfailureは特定scenarioだけではなく、priority relationのcombinatorial complexityと関連している。

## Reasoning Effort

GPT-5.4ではreasoning effortを増加させるとperformanceが **15.5% → 60.9%** まで大きく改善する。

これはmany-tier privilege resolutionが単なるalignment/refusal policyではなく、一定程度 **inference-time reasoning problem** でもあることを示唆する。一方60.9%でも十分ではなく、reasoning tokenを増やすだけで解決するわけではない。

## Representation Sensitivity

同一priority orderingでもprivilege representationをordinalからscalar形式へ変更すると、GPT-5.4で **8.4ポイント**低下する。

もしモデルが純粋にordering relationを理解していれば表記変更の影響は小さいはずである。この結果は、最新modelでもpriority reasoningがrepresentation-specific heuristicsへ依存していることを示す。

## Security Interpretation

実際のattackではattackerが高priorityを獲得する必要はなく、モデルに「この低priority instructionの方が優先される」と誤認させればよい。many-tier environmentでpriority reasoningが不安定なら、role confusion、priority spoofing、nested tool / sub-agent経由のinstruction injectionが新しいattack surfaceとなる。

したがってManyIHは直接attackを提案する論文ではないものの、privilege escalation attackが成立しうるmodel-level weaknessを測るbenchmarkと位置づけられる。

## Limitations

benchmark上のexplicit tierは実際のproductのauthority modelより単純化されている。現実には権限は全順序ではなく、resource / actionごとに異なるpartial orderやcapabilityとして定義されることがある。

またagent harnessがcryptographically sourceを認証できても、LLMがそのmetadataを正しくreasoningできるかは別問題である。本benchmarkは主に後者を測る。

arXiv公開直後でありcitation数について信頼できるsourceを確認できなかったため、本repositoryでは未確認として記録する。

## LLM Model Security Taxonomyでの位置づけ

```text
2-level / fixed-role hierarchy
  system > user
        ↓
4-level fixed-role evaluation
  IHEval
        ↓
Arbitrary many-tier privilege reasoning
  ManyIH-Bench  ← 本論文
        ↓
Future: partial-order / capability-based authority
```

本論文はInstruction Hierarchyをprompt-injection defenseから、より一般的な **authorization reasoning** の問題へ広げる位置にある。

## 実装・研究上の示唆

agent architectureではrole labelだけをLLMへ渡すのではなく、権限metadataを明示し、可能ならLLM外部のdeterministic policy engineでもauthorizationを強制する必要がある。ManyIHの結果は、model-only privilege enforcementを多数tierへ拡張することの危険性を示す。

Defense研究としては、[Instructional Segment Embedding](../Defense/2024-2410.09102-instructional-segment-embedding.md) のようなarchitecture-level privilege representationをmany-tierへ拡張することや、priority orderingをtraining distribution外へgeneralizeさせるcurriculum / synthetic data設計が自然な方向となる。

## 関連研究

- [The Instruction Hierarchy](../Defense/2024-2404.13208-instruction-hierarchy.md) — privileged instruction trainingの基盤。
- [IHEval](2025-2502.08745-iheval.md) — fixed system/user/history/tool hierarchyの包括的benchmark。
- [Instructional Segment Embedding](../Defense/2024-2410.09102-instructional-segment-embedding.md) — privilege sourceをembeddingとして明示するarchitecture-level defense。

## Code

Official repository: https://github.com/amazon-science/manyih
