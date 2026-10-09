# LLM Model Security — Attack

Instruction Hierarchy / Instruction Priority / Privilege Separationを破壊・回避するattack論文の要約と更新履歴。

## 更新履歴

### 2026-10-09

- **[Backdoor-Powered Prompt Injection Attacks Nullify Defense Methods](2025-2510.03705-backdoor-powered-prompt-injection.md)** — cited: **未確認**（venue: **Findings of EMNLP 2025**）  
  SFT poisoningでtriggered backdoorを形成し、StruQ / SecAlignによる後続defense後も残るprivilege-separation脆弱性を評価。


### 2026-10-06

- **[ChatInject: Abusing Chat Templates for Prompt Injection in LLM Agents](2025-2509.22830-chatinject.md)** — cited: **16**（citation source: **Lune**; venue: **ICLR 2026**）  
  low-trust tool output内にnative chat-templateのsystem/user/assistant role structureを偽造するpriority/role spoofing attack。Multi-turn persuasion、cross-model transfer、Mixture-of-Templatesによりunknown-templateのblack-box settingにも拡張する。


### 2026-10-05

- **[Will the User Ever Know? Covert Indirect Prompt Injection Attacks on Tool-Using LLM Agents](2026-2608.30362-icoa.md)** — cited: **未確認**（venue fallback: **EMNLP 2026 Main**）  
  malicious tool action後にRETURN anchorでoriginal taskへ戻り、最終回答から攻撃痕跡を隠すblack-box ICoA。ASRをCovert Success Rate / Overt Success Rateへ分解し、User > Tool violation後のuser observabilityを評価する。


### 2026-09-28

- **[PISmith: Reinforcement Learning-based Red Teaming for Prompt Injection Defenses](2026-2603.13026-pismith.md)** — cited: **≥1**（citation source: **directly verified citing paper (SecOPD); aggregate count unavailable**）  
  black-box RLによるadaptive prompt-injection red teaming。reward sparsity下でも探索を維持し、Meta SecAlign等のDefenseをadaptiveに評価する。


### 2026-09-21

- **[Just Ask: Curious Code Agents Reveal System Prompts in Frontier LLMs](2026-2601.21233-just-ask.md)** — cited: **未確認**（venue fallback: **ICML 2026**）  
  system prompt extractionをblack-box online explorationとして扱い、UCBでattack skillを探索・再利用するadaptive attack。gradient/logit accessなしでhigher-privilege promptのconfidentialityを攻撃する。

### 2026-09-20

- **[Prompt Injection as Role Confusion](2026-2603.12277-prompt-injection-role-confusion.md)** — cited: **未確認**（venue fallback: **ICML 2026**）  
  prompt injectionをlatent role confusionとして分析し、hidden-state Role ProbeでUserness / Assistantness / CoTnessを測定。低権限textへmodel自身のreasoningらしい偽CoTを埋め込むCoT Forgeryにより、gradient不要のrole/priority spoofing attackを提示する。

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
