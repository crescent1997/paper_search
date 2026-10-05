# LLM Model Security — Benchmark

Instruction Hierarchy / Instruction Priority / Privilege Separationの遵守能力、競合instructionへの挙動、prompt injection / system prompt extraction等へのrobustnessを測るbenchmark・diagnostic・survey論文の要約と更新履歴。

## 更新履歴

### 2026-10-05

- **[Rethinking Assessments of Prompt Injection Attacks](2026-acl-findings-1191-rethinking-prompt-injection-assessments.md)** — cited: **未確認**（venue fallback: **Findings of ACL 2026**）  
  8 evaluation settings / 37 applications / 185 injected tasks / 21 attack instructions / 143,745 queriesでprompt-injection attack・Defenseのexternal validityを再評価。in-lab結果がreal-world applicationへ一般化しない問題を分析する。


### 2026-09-29

- **[IH-Benchmark: A Conflict-Centered Benchmark for Instruction-Hierarchy Robustness in LLM Applications](2026-2607.25987-ih-benchmark.md)** — cited: **1**（citation source: **Scholar Feed in-corpus citations**）  
  2,336 scenarios / 44 constraint familiesでSystem > UserとUser > Toolを分離評価。37 modelでcomplianceが20.5–98.2%に広がり、S>U robustnessがU>Tを予測しないことやconstraint hardening / subtle tool injectionのfailureを分析する。


### 2026-09-28

- **[Language Shapes Instruction Hierarchy Compliance in Multilingual LLMs](2026-2607.23545-xih-bench.md)** — cited: **未確認**（venue fallback: **EMNLP 2026 Main**）  
  6言語でInstruction Hierarchyを評価するXIH-Bench。言語依存のpriority asymmetryとLanguage Boundary Effectを分析する。


### 2026-09-20

- **[Control Illusion: The Failure of Instruction Hierarchies in Large Language Models](2025-2502.15851-control-illusion.md)** — cited: **未確認**（venue fallback: **AAAI 2026 Main Technical Track**）  
  system/user roleをswapした相互排他的constraint pairでpriority followingをcontrolledに評価。role designationよりconstraint固有biasやauthority / expertise / consensus等のsocial hierarchy cueが強く作用する場合を示す。

### 2026-09-16

- **[IHEval: Evaluating Language Models on Following the Instruction Hierarchy](2025-2502.08745-iheval.md)** — cited: **19**（citation source: **Hugging Face paper metadata**）  
  system > user > conversation history > tool outputの4-level hierarchyを3,538 examples / 9 tasksで評価。Reference / Aligned / Conflictを比較し、GPT-4oを含む強力なLLMでもconflict時に大幅な性能低下が残り、instruction strictness等のsurface cueにpriority判断が影響されることを示す。

- **[Many-Tier Instruction Hierarchy in LLM Agents](2026-many-tier-instruction-hierarchy.md)** — cited: **未確認**（citation source: **未確認**）  
  roleとprivilegeを分離し、853 tasks / 46 agent scenarios / 最大12 privilege tiersでarbitrary priority reasoningを評価するManyIH-Bench。2-tierで99%以上の最新modelもmany-tierでは40%前後まで低下し、priority表記やreasoning effortにも強く依存することを示す。
