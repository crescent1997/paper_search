# LLM Model Security — Attack

Instruction Hierarchy / Instruction Priority / Privilege Separationを破壊・回避するattack論文の要約と更新履歴。

## 更新履歴

### 2026-09-18

- **[The Attacker Moves Second: Stronger Adaptive Attacks Bypass Defenses Against LLM Jailbreaks and Prompt Injections](2025-2510.09023-attacker-moves-second.md)** — cited: **38**（citation source: **Semantic Scholar**）  
  GCG等の既存attackを固定設定で当てるだけでは不十分として、gradient / RL / random search / human-guided explorationをdefenseごとに適応させるadaptive attack methodologyを提示。StruQやMeta SecAlignなど、static評価では低ASRだったprompt-injection defenseを大幅に突破し、defense-aware evaluationの必要性を示す。
