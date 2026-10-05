---
title: "Rethinking Assessments of Prompt Injection Attacks"
summary: "従来のprompt-injection評価が単純なin-lab target task・compliant injected task・固定attack instructionへ偏る問題を、8 settings、37 real-world applications、185 injected tasks、21 attack instructions、143,745 queriesで再評価。prompt-level/model-level defenseのdeployment generalizationも検証する。"
authors_affiliations: "Chi Cui, Yixin Wu, Michael Backes, Yang Zhang"
published: "2026-07"
publication_status: "Findings of ACL 2026"
topics: ["LLM Security", "Prompt Injection", "Benchmark", "Evaluation", "Model-level Defense", "Real-world Applications"]
cited: null
citation_source: null
url: "https://aclanthology.org/2026.findings-acl.1191/"
code: "https://github.com/TrustAIRLab/Prompt_Injection_Assessment"
last_checked: "2026-10-05"
---

# Rethinking Assessments of Prompt Injection Attacks

## 概要

本論文はprompt injection attack / defenseの評価がlimited in-lab settingへ依存し、real-world deploymentへ一般化しない問題を大規模に検証する。

評価frameworkを3軸へ分解する。

```text
Target Task
  in-lab / in-the-wild

Injected Task
  compliant / non-compliant

Attack Instruction
  simple / complex
```

2×2×2で8 evaluation settingsとなる。

## 評価規模

- 37 target tasks / real-world applications
- 185 injected tasks
- 21 attack instructions
- 143,745 queries
- 8 evaluation settings

従来の単純なclassification / generation benchmarkより大幅にscenario diversityを広げる。

## Target Task

### In-lab

classic NLP task等、topicやexpected outputが比較的明確な研究用setting。

### In-the-wild

real-world LLM-integrated applicationを想定し、system instructionやoutput spaceが複雑。

実験ではin-the-wild targetがよりprompt injectionに脆弱な傾向を示す。

## Injected Task

### Compliant

usage policyには反しないがoriginal application behaviorを逸脱させるtask。

### Non-compliant

harmful / unethical output等、model safety policyにも反するtask。

non-compliant injectionではprompt-injection resistanceとsafety alignmentが相互作用する。

## Attack Instruction

### Simple

従来研究で使われる直接的なoverride / ignore instruction。

### Complex

safety / instruction defenseを回避するためにより複雑化したattack instruction。

興味深いことに、complex attackが常に強いわけではなくsimple attackの方が有効な場合がある。

paperはsimple instructionが通常user inputに近く、internal defenseをtriggerしにくい可能性を指摘する。

## Main Findings

### Real-world applications are more vulnerable

in-the-wild tasksはin-lab tasksよりprompt injection成功率が高い。

research benchmarkだけで得たASRをdeployment securityへそのまま外挿できない。

### Complex attack is not necessarily stronger

attack sophisticationとASRは単調ではない。

複雑なattack instructionはsecurity mechanismに検出されやすくなる場合がある。

### Non-compliant task interaction

model built-in safetyによりnon-compliant injected taskはcompliant taskとは異なるbehaviorを示す。

したがってprompt injection Defense評価でattack objectiveのpolicy statusを統制する必要がある。

### Model-specific vulnerable tasks

複数LLMで特定non-compliant injected taskへの脆弱性が残り、model capabilityだけでは一様に解消されない。

## Defense Evaluation

prompt-levelとmodel-level Defenseを同じextended frameworkで再評価する。

重要な結論は、restricted research settingで有効なDefenseがreal-world application / attack variationでも同程度にrobustとは限らないことである。

prompt-level Defenseは特にbase model capabilityへ依存する。

## Internal Representation Analysis

empirical ASRだけでなく、prompt injectionがmodel internal representationへ与える変化も分析する。

これによりattack successをsurface outputだけでなくmodel内部のbehavior shiftから理解しようとする。

## Instruction Hierarchyとの関係

[IHEval](2025-2502.08745-iheval.md)や[IH-Benchmark](2026-2607.25987-ih-benchmark.md)はhigher/lower-priority instruction conflictそのものを測る。

本論文は、

```text
priority failure
   ↓
prompt injection attack / defense
   ↓
evaluation condition changes
```

に焦点を置く。

つまりIH capability benchmarkではなく、**prompt injection security claimのexternal validityを測るmeta-evaluation**として位置づけられる。

## StruQ / SecAlign系への示唆

[StruQ](../Defense/2024-2402.06363-struq.md)、[SecAlign](../Defense/2024-2410.05451-secalign.md)、[Meta SecAlign](../Defense/2025-2507.02735-meta-secalign.md)等のDefenseを評価するとき、単一target task / attack templateだけでは不十分である。

少なくとも、

1. in-lab / in-the-wild
2. compliant / non-compliant injected goal
3. simple / complex injection
4. multiple base models

でsliceすべきである。

## Adaptive Attackとの関係

本frameworkはevaluation diversityを広げるが、Defense-aware optimizationそのものとは異なる。

[The Attacker Moves Second](../Attack/2025-2510.09023-attacker-moves-second.md)や[PISmith](../Attack/2026-2603.13026-pismith.md)はDefenseを固定した後にattackerを再最適化する。

```text
Rethinking Assessments:
 broaden evaluation distribution

Adaptive attacks:
 optimize against the specific Defense
```

両方を組み合わせることでより厳しい評価になる。

## ICoAとの関係

[ICoA](../Attack/2026-2608.30362-icoa.md)はASRをCSR / OSRへ分解し、attack成功後のuser observabilityを新たな評価軸にする。

本論文がtask / attack / application diversityを広げるのに対し、ICoAは**success metric自体を拡張する**。

## Limitations

- 37 applicationsでもreal-world application space全体は網羅しない。
- attack instructionsは有限集合。
- adaptive optimizationをDefenseごとに最大化するframeworkではない。
- model/API updateで結果が変化する。
- evaluation judge / task-specific success criteriaに誤差があり得る。
- agentic tool-useのauthorization failureを全面的に扱うbenchmarkではない。
- IH role hierarchyを直接diagnoseするbenchmarkではない。

## LLM Model Security Taxonomyでの位置づけ

```text
Hierarchy capability:
 IHEval / IH-Benchmark

Prompt-injection robustness:
 Rethinking Assessments
  -> broader task/attack/application distribution

Worst-case robustness:
 Attacker Moves Second / PISmith

User observability:
 ICoA CSR / OSR
```

## 実装・研究上の示唆

prompt injection paperでは単一ASRだけでなくevaluation matrixを明示すべきである。

最低でもtarget task realism、injected goal safety status、attack complexity、base model、Defense typeを分離し、平均値だけでなくslice別failureを報告することが望ましい。

## 関連研究

- [IHEval](2025-2502.08745-iheval.md)
- [IH-Benchmark](2026-2607.25987-ih-benchmark.md)
- [Control Illusion](2025-2502.15851-control-illusion.md)
- [StruQ](../Defense/2024-2402.06363-struq.md)
- [SecAlign](../Defense/2024-2410.05451-secalign.md)
- [Meta SecAlign](../Defense/2025-2507.02735-meta-secalign.md)
- [The Attacker Moves Second](../Attack/2025-2510.09023-attacker-moves-second.md)
- [PISmith](../Attack/2026-2603.13026-pismith.md)
- [ICoA](../Attack/2026-2608.30362-icoa.md)
