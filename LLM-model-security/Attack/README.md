# LLM Model Security — Attack

Instruction Hierarchy / Instruction Priority / Privilege Separationを破壊・回避するattack論文の要約と更新履歴。

## 更新履歴

### 2026-09-19

- ★ **[Universal and Transferable Adversarial Attacks on Aligned Language Models](2023-2307.15043-gcg.md)** — cited: **1230**（citation source: **arXiv.gg**）  
  Greedy Coordinate Gradient (GCG)でadversarial suffixを離散最適化し、複数prompt・複数modelへ共通化してblack-box modelへのtransferを実現するoptimization-based jailbreakの基礎。SecAlign系を含む後続Defenseのstress testにつながる技術的祖先。

- **[Faster-GCG: Efficient Discrete Optimization Jailbreak Attacks against Aligned Large Language Models](2024-2410.15362-faster-gcg.md)** — cited: **35**（citation source: **alphaXiv**）  
  GCGのgradient近似誤差、top-k sampling、重複suffix評価を改善し、約8×のsample efficiency・約7×のwall-clock speedupを報告。固定budgetでのDefense評価をより厳しくする後続attack。

- **[SlotGCG: Exploiting the Positional Vulnerability in LLMs for Jailbreak Attacks](2026-2606.05609-slotgcg.md)** — cited: **未確認**（venue fallback: **ICLR 2026**）  
  adversarial tokensをsuffixへ固定せず、Vulnerable Slot Scoreで脆弱な挿入位置を選んでからGCG型optimizationを行う。attack contentだけでなくinjection positionもadaptive variableにする。

### 2026-09-18

- **[The Attacker Moves Second: Stronger Adaptive Attacks Bypass Defenses Against LLM Jailbreaks and Prompt Injections](2025-2510.09023-attacker-moves-second.md)** — cited: **38**（citation source: **Semantic Scholar**）  
  GCG等の既存attackを固定設定で当てるだけでは不十分として、gradient / RL / random search / human-guided explorationをdefenseごとに適応させるadaptive attack methodologyを提示。StruQやMeta SecAlignなど、static評価では低ASRだったprompt-injection defenseを大幅に突破し、defense-aware evaluationの必要性を示す。
