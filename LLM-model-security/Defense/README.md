# LLM Model Security — Defense

Instruction Hierarchy / Instruction Priority / Privilege SeparationをLLM自身に学習・内部化させ、低権限instructionによるprompt injection、jailbreak、system prompt extraction等を防ぐdefense論文の要約と更新履歴。model-level手法との比較上重要なsystem-level privilege separationも、その旨を明記して収録する。

## 更新履歴

### 2026-09-18

- ★ **[Meta SecAlign: A Secure Foundation LLM Against Prompt Injection Attacks](2025-2507.02735-meta-secalign.md)** — cited: **62**（citation source: **Semantic Scholar**）  
  SecAlignをSecAlign++へ拡張し、dedicated input role、delimiter filtering、preference optimization、injection position randomization等を組み合わせてopen-weight foundation LLMへprompt/data privilege separationを組み込む。static agent benchmarkでは低ASRを達成する一方、white-box GCGやadaptive attackには残存脆弱性がある。

- **[Reasoning Up the Instruction Ladder for Controllable Language Models](2025-2511.04694-reasoning-up-instruction-ladder.md)** — cited: **未確認**（venue fallback: **Findings of ACL 2026**）  
  instruction hierarchy resolutionをexplicit reasoning taskとして扱い、VerIHとRLVR / GRPOでsystem-user conflictを学習する。IHEval Conflictを大幅に改善し、security-specific trainingなしでもjailbreak / prompt-injection robustnessへgeneralizeする。

### 2026-09-17

- **[StruQ: Defending Against Prompt Injection with Structured Queries](2024-2402.06363-struq.md)** — cited: **未確認**  
  trusted promptとuntrusted dataをstructured queryとして分離し、secure front-endとstructured instruction tuningでdata側のinstructionを無視させる。後続Instruction Hierarchyにつながる2-level privilege separationの代表的手法。

- **[SecAlign: Defending Against Prompt Injection with Preference Optimization](2024-2410.05451-secalign.md)** — cited: **未確認**  
  StruQ型SFTのpositive-only objectiveを拡張し、secure / insecure response pairをDPOで直接比較する。optimization-based prompt injectionへのrobustnessを大きく改善する。

- **[Defeating Prompt Injections by Design](2025-2503.18813-defeating-prompt-injections-by-design-camel.md)** — cited: **未確認**  
  CaMeLによるsystem-level defense。Privileged / Quarantined LLMとcapability・provenance trackingを用い、LLMがprompt injectionされてもuntrusted computationへ高権限を渡さない。model-level hierarchyとの比較対象として収録。

### 2026-09-16

- **[Instructional Segment Embedding: Improving LLM Safety with Instruction Hierarchy](2024-2410.09102-instructional-segment-embedding.md)** — cited: **23**（citation source: **alphaXiv**）  
  system / user / untrusted data等のinstruction sourceをlearnable segment embeddingとして各tokenへ付与し、priorityをarchitecture-level signalとして明示する。Instruction Hierarchyのbehavioral trainingを補完し、prompt injection / extractionへのrobustnessと通常のinstruction followingを同時に改善する。

### 2026-09-15

- ★ **[The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions](2024-2404.13208-instruction-hierarchy.md)** — cited: **158**（citation source: **Pith**）  
  system > user > tool / third-partyというinstruction privilegeを明示し、alignedな低権限instructionは従いつつconflictingな低権限instructionだけを無視するようsynthetic dataで学習する。GPT-3.5 Turboでprompt injection、system prompt extraction、未学習jailbreak等へのrobustnessを大幅に改善し、LLM model側にprivilege separationを持たせる代表的研究。
