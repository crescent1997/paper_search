# RAG Security Paper Notes

LLM / Retrieval-Augmented Generation (RAG) のsecurity riskに関する論文要約の更新履歴。

特に、private retrieval database、knowledge base、corpus、retrieved documentsのdata extraction / stealing / exfiltrationを重点的に収集する。

## 更新履歴

### 2026-09-10

- **[Benchmarking Knowledge-Extraction Attack and Defense on Retrieval-Augmented Generation](Benchmark/2026-2602.09319-benchmarking-knowledge-extraction-attack-and-defense.md)** — Google Scholar cited: **未確認**  
  RAG knowledge-base extraction attack / defenseを複数retriever・generator・indexing方式・dataset・metricで統一比較し、retrievalとgenerationのどこで漏洩するかを分離評価するsystematic benchmark。

- **[SafeRAG: Benchmarking Security in Retrieval-Augmented Generation of Large Language Model](Benchmark/2025-2501.18636-saferag.md)** — Google Scholar cited: **未確認**  
  Silver Noise、Inter-context Conflict、Soft Ad、White DoSの4種のattack scenarioを用い、retriever・filter・generatorを横断してRAG security robustnessを評価するACL 2025 benchmark。

- **[Silent Leaks: Implicit Knowledge Extraction Attack on RAG Systems through Benign Queries](Attack/2025-2505.15420-silent-leaks-ikea.md)** — Google Scholar cited: **未確認**  
  prompt injectionやjailbreakを使わず、benign-looking queriesをadaptiveに生成してRAG内部knowledgeを抽出するIKEAを提案。input/output filteringを回避しやすく、substitute RAG構築まで評価する。

### 2026-09-09

- **[Feedback-Guided Extraction of Knowledge Base from Retrieval-Augmented LLM Applications](Attack/2024-2411.14110-feedback-guided-extraction-copybreakrag.md)** — Google Scholar cited: **未確認**  
  初期版RAG-Thiefを発展させたCopyBreakRAG。抽出済みchunkをfeedback memoryとして利用し、exploration / exploitationを切り替えながらblack-box RAGのknowledge baseを大規模に復元する。

- **[Differentially Private Synthetic Text Generation for Retrieval-Augmented Generation (RAG)](Defense/2025-2510.06719-dp-synrag.md)** — Google Scholar cited: **未確認**  
  private corpusをone-timeのDP synthetic corpusへ変換し、その後のqueryでは追加privacy budgetを消費せず通常のRAGとして利用するdata-layer defenseを提案する。

- ★ **[PoisonedRAG: Knowledge Corruption Attacks to Retrieval-Augmented Generation of Large Language Models](Attack/2024-2402.07867-poisonedrag.md)** — Google Scholar cited: **193**  
  knowledge baseへ少数のmalicious textsを注入し、target queryに対してattacker-chosen answerを生成させるknowledge corruption attack。大規模databaseでも高いASRを示す。

### 2026-09-08

- ★ **[The Good and The Bad: Exploring Privacy Issues in Retrieval-Augmented Generation (RAG)](Attack/2024-2402.16893-the-good-and-the-bad.md)** — Google Scholar cited: **336**  
  RAGがprivate retrieval databaseという新しいprivacy attack surfaceを作る一方、LLM本体のtraining-data leakageを緩和する可能性もあることを、HealthCareMagic / Enron Email等を用いたtargeted・untargeted extractionで体系的に評価する。

- ★ **[Follow My Instruction and Spill the Beans: Scalable Data Extraction from Retrieval-Augmented Generation Systems](Attack/2024-2402.17840-follow-my-instruction-and-spill-the-beans.md)** — Google Scholar cited: **111**  
  instruction-following能力を悪用してretrieved contextをほぼverbatimに再出力させるblack-box extraction attackを示し、customized GPTやmultiple-query corpus reconstructionまで評価する。

- **[Data Extraction Attacks in Retrieval-Augmented Generation via Backdoors](Attack/2024-2411.01705-data-extraction-attacks-via-backdoors.md)** — Google Scholar cited: **37**  
  fine-tuning dataへ少量のpoisonを混入してgeneratorへbackdoorを埋め込み、trigger時にretrieved private documentをverbatim / paraphraseして漏洩させるsupply-chain型attackを提案する。

- **[Connect the Dots: Knowledge Graph-Guided Crawler Attack on Retrieval-Augmented Generation Systems](Attack/2026-2601.15678-connect-the-dots.md)** — Google Scholar cited: **2**  
  black-box RAGのresponseからKnowledge Graphを構築し、未探索領域をglobal planningで選択するRAGCrawlerにより、限られたquery budgetでprivate knowledge baseを効率的にstealする。

## Citation policy

被引用数は **Google Scholar** をsourceとし、各Markdownの `last_checked` 時点の値を記録する。Google Scholar cited >= 50 の論文には ★ を付与する。Google Scholarの値を信頼できる形で取得できなかった場合は、推測値を入れず **未確認** と明記し、次回確認時に更新する。
