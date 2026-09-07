# RAG Security Paper Notes

LLM / Retrieval-Augmented Generation (RAG) のsecurity riskに関する論文要約の更新履歴。

特に、private retrieval database、knowledge base、corpus、retrieved documentsのdata extraction / stealing / exfiltrationを重点的に収集する。

## 更新履歴

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

被引用数は **Google Scholar** をsourceとし、各Markdownの `last_checked` 時点の値を記録する。Google Scholar cited >= 50 の論文には ★ を付与する。
