# RAG Security — Attack

RAGに対するattack論文の要約と更新履歴。private retrieval database / knowledge base / corpus / retrieved documentsのdata extraction・stealing・exfiltrationを特に重視する。

## 更新履歴

### 2026-09-24

- **[ImageAuditor: Membership Inference Attack against Image-based Retrieval-Augmented Generation](2026-2606.03354-imageauditor.md)** — cited: **3**（citation source: **alphaXiv**）  
  Image-based RAGのopaque retrieval databaseにcandidate imageが含まれるかを推定するmembership inference attack。RGPOでcross-modal retrieval queryを最適化し、task-specific extractionとmulti-query aggregationでmembership signalを取り出す。

### 2026-09-22

- **[RIPRAG: Hack a Black-box Retrieval-Augmented Generation Question-Answering System with Reinforcement Learning](2025-2510.10008-riprag.md)** — cited: **未確認**（citation source: **未確認**）  
  target RAGをblack-box RL environmentとして扱い、最終QA feedbackからpoison generatorを適応させるknowledge-poisoning attack。BRPOとretrieval-aware rewardにより、retriever/reranker内部を知らずcomplex retrieval pipelineへ適応する。Findings of ACL 2026。

### 2026-09-16

- **[E-MIA: Exam-Style Black-Box Membership Inference Attacks against RAG Systems](2026-2605.00955-e-mia.md)** — cited: **1**（citation source: **arXiv.gg**）  
  candidate documentからhard evidenceを抽出して4種類のexam questionsへ変換し、複数問題の正答をaggregateするblack-box membership inference。semantic-similarity型MIAよりmember/non-memberを分離しやすく、低FPR領域とprompt guardrail下でも高いattack performanceを示す。

### 2026-09-15

- **[Unleashing Worms and Extracting Data: Escalating the Outcome of Attacks against RAG-based Inference in Scale and Severity Using Jailbreaking](2024-2409.08045-unleashing-worms-extracting-data.md)** — cited: **2**（citation source: **SciSpace**）  
  jailbreakでretrieved documentsを露出させつつ、embedding-spaceをadaptiveに探索してRAG databaseを高率に抽出するDGEAを提案。さらにself-replicating promptによるRAG wormで攻撃を複数applicationへ伝播させる。

### 2026-09-12

- **[Exposing Privacy Risks in Graph Retrieval-Augmented Generation](2025-2508.17222-exposing-privacy-risks-graphrag.md)** — cited: **2**（citation source: **ResearchGate**）  
  GraphRAGに対するblack-box extractionを体系評価し、raw source textの漏洩を抑えられる場合がある一方、entity / relationship / descriptionなど内部Knowledge Graphのstructured knowledgeが高率に漏洩するprivacy trade-offを示す。Findings of ACL 2026。

### 2026-09-11

- **[Fine-Grained Privacy Extraction from Retrieval-Augmented Generation Systems via Knowledge Asymmetry Exploitation](2025-2507.23229-fine-grained-privacy-extraction.md)** — cited: **未確認**（citation source: **未確認**）  
  target RAGとstandard LLMのknowledge asymmetryを利用し、混在したRAG responseからprivate knowledge-base由来sentenceをfine-grainedに特定するblack-box privacy extraction attack。ICLR 2026。

- **[Riddle Me This! Stealthy Membership Inference for Retrieval-Augmented Generation](2025-2502.00306-riddle-me-this.md)** — cited: **33**（citation source: **ResearchGate**）  
  target documentから自然なinterrogation queryを生成し、複数回答の正答性を集約してprivate RAG databaseへのdocument membershipを高精度かつstealthyに推定する。ACM CCS 2025。

### 2026-09-10

- **[Silent Leaks: Implicit Knowledge Extraction Attack on RAG Systems through Benign Queries](2025-2505.15420-silent-leaks-ikea.md)** — cited: **未確認**（citation source: **未確認**）  
  prompt injectionやjailbreakを使わず、benign-looking queriesをadaptiveに生成してRAG内部knowledgeを抽出するIKEAを提案。input/output filteringを回避しやすく、substitute RAG構築まで評価する。

### 2026-09-09

- **[Feedback-Guided Extraction of Knowledge Base from Retrieval-Augmented LLM Applications](2024-2411.14110-feedback-guided-extraction-copybreakrag.md)** — cited: **未確認**（citation source: **未確認**）  
  初期版RAG-Thiefを発展させたCopyBreakRAG。抽出済みchunkをfeedback memoryとして利用し、exploration / exploitationを切り替えながらblack-box RAGのknowledge baseを大規模に復元する。

- ★ **[PoisonedRAG: Knowledge Corruption Attacks to Retrieval-Augmented Generation of Large Language Models](2024-2402.07867-poisonedrag.md)** — cited: **193**（citation source: **Google Scholar**）  
  knowledge baseへ少数のmalicious textsを注入し、target queryに対してattacker-chosen answerを生成させるknowledge corruption attack。大規模databaseでも高いASRを示す。

### 2026-09-08

- ★ **[The Good and The Bad: Exploring Privacy Issues in Retrieval-Augmented Generation (RAG)](2024-2402.16893-the-good-and-the-bad.md)** — cited: **336**（citation source: **Google Scholar**）  
  RAGがprivate retrieval databaseという新しいprivacy attack surfaceを作る一方、LLM本体のtraining-data leakageを緩和する可能性もあることを、HealthCareMagic / Enron Email等を用いたtargeted・untargeted extractionで体系的に評価する。

- ★ **[Follow My Instruction and Spill the Beans: Scalable Data Extraction from Retrieval-Augmented Generation Systems](2024-2402.17840-follow-my-instruction-and-spill-the-beans.md)** — cited: **111**（citation source: **Google Scholar**）  
  instruction-following能力を悪用してretrieved contextをほぼverbatimに再出力させるblack-box extraction attackを示し、customized GPTやmultiple-query corpus reconstructionまで評価する。

- **[Data Extraction Attacks in Retrieval-Augmented Generation via Backdoors](2024-2411.01705-data-extraction-attacks-via-backdoors.md)** — cited: **37**（citation source: **Google Scholar**）  
  fine-tuning dataへ少量のpoisonを混入してgeneratorへbackdoorを埋め込み、trigger時にretrieved private documentをverbatim / paraphraseして漏洩させるsupply-chain型attackを提案する。

- **[Connect the Dots: Knowledge Graph-Guided Crawler Attack on Retrieval-Augmented Generation Systems](2026-2601.15678-connect-the-dots.md)** — cited: **2**（citation source: **Google Scholar**）  
  black-box RAGのresponseからKnowledge Graphを構築し、未探索領域をglobal planningで選択するRAGCrawlerにより、限られたquery budgetでprivate knowledge baseを効率的にstealする。
