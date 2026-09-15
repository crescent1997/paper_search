# LLM Model Security — Defense

Instruction Hierarchy / Instruction Priority / Privilege SeparationをLLM自身に学習・内部化させ、低権限instructionによるprompt injection、jailbreak、system prompt extraction等を防ぐdefense論文の要約と更新履歴。

## 更新履歴

### 2026-09-15

- ★ **[The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions](2024-2404.13208-instruction-hierarchy.md)** — cited: **158**（citation source: **Pith**）  
  system > user > tool / third-partyというinstruction privilegeを明示し、alignedな低権限instructionは従いつつconflictingな低権限instructionだけを無視するようsynthetic dataで学習する。GPT-3.5 Turboでprompt injection、system prompt extraction、未学習jailbreak等へのrobustnessを大幅に改善し、LLM model側にprivilege separationを持たせる代表的研究。
