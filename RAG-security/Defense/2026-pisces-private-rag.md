---
title: "Pisces: Cryptography-based Private Retrieval-Augmented Generation with Dual-Path Retrieval"
summary: "semantic retrievalとlexical retrievalの両経路をMPC / PIR / FHE / PSI等でprivateに実行し、user queryとserver-side knowledge base双方を秘匿したままhybrid RAG retrievalを行うcryptographic framework。"
authors_affiliations: "Xiaojian Liang, et al.（Ant International / Ant Group）"
published: "2026"
publication_status: "ICLR 2026 Conference Paper"
topics: ["RAG Security", "Private Retrieval", "Cryptography", "MPC", "PIR", "FHE", "PSI", "Dual-Path Retrieval"]
cited: null
citation_source: null
url: null
code: "https://github.com/liang-xiaojian/Pisces"
last_checked: "2026-09-14"
---

# Pisces: Cryptography-based Private Retrieval-Augmented Generation with Dual-Path Retrieval

> RAG retrievalをsemi-honest participants間で実行する際、user queryとserver-side knowledge baseの双方を秘匿するcryptographic private RAG framework。Dense semantic retrievalだけでなくBM25型lexical retrievalもprivateに実行する **dual-path retrieval** を設計し、大規模corpusでの暗号計算・通信costをcoarse-to-fine filteringとmulti-instance labeled PSIによって削減する。

## 概要

RAG privacyでは、external attackerがblack-box APIからprivate corpusをextractする問題だけでなく、retrieval infrastructureそのものを信頼できない場合のprivacyも重要になる。例えばenterprise knowledge baseを外部cloud / service provider上で検索する場合、userはqueryをserverへ見せたくなく、knowledge-base owner側もcorpus全体をuserへ開示したくない。

Piscesはこの **private retrieval computation** を対象とする。従来のcryptographic private RAGはdense semantic retrievalを中心とするものが多いが、実用RAGではsemantic similarityだけでなくBM25等のlexical matchingを組み合わせるhybrid retrievalがaccuracy上重要である。

Piscesはsemantic pathとlexical pathの双方をsecure computationとして実装し、最後にTop-K retrieval resultsだけをprivacy-preservingに取得する。Semantic pathではcoarse-to-fine encrypted retrieval、Lexical pathではmulti-instance labeled PSIを導入し、暗号化retrievalの主要bottleneckを削減する。

## 問題設定 / Threat Model

Piscesは主に **semi-honest adversary** を想定する。participantsはprotocol specificationには従うが、protocol execution中に観測できるmessage / intermediate informationから相手のprivate inputを推測しようとする。

保護対象は双方に存在する。

### User Privacy

serverはuser queryそのものやquery embedding、query terms、最終的にどのdocumentがretrievedされたかを知るべきではない。

queryはuser intentやmedical / financial / enterprise informationを含み得るため、retrieval serverへのquery disclosure自体がprivacy riskとなる。

### Knowledge-Base Privacy

userはserverのknowledge base全体、document embeddings、inverted index、非retrieved documents等を知るべきではない。

userが取得できるのはprotocolで許可されたTop-K retrieval resultsに限定される。

### Scope

このthreat modelは [Feedback-Guided Extraction / CopyBreakRAG](../Attack/2024-2411.14110-feedback-guided-extraction-copybreakrag.md) や [RAGFort](2025-2511.10128-ragfort.md) のようなpublic RAG APIへのmalicious queryとは異なる。

Piscesは主に **retrieval computationを実行するparticipants間のconfidentiality** を扱い、generator responseからknowledge baseをreconstructするattackを直接防ぐものではない。

## 先行研究からの改善点

### Semantic-only Private Retrievalの限界

既存private RAGではdense embedding similarityをsecure computationで計算する方式が中心である。しかし実際のRAGではexact entity、rare term、ID、technical keyword等についてlexical retrievalが重要であり、semantic-only retrievalはplaintext hybrid retrievalよりaccuracyが落ちる場合がある。

Piscesはsemantic + lexicalをprivateに実行する **dual-path retrieval** を導入する。

### Full Secure Similarity SearchのCost

corpus内の全chunkに対してsecure cosine similarityを計算すると、document数に比例してMPC costとcommunicationが増える。

PiscesはSemantic pathをcoarse / fineの2段階へ分け、cheapなsecure Hamming filteringでcandidateを削減してからexpensiveなcosine similarityを計算する。

### Document-wise PSIのCost

BM25 lexical retrievalではquery termと各document termのintersection / term frequencyをsecureに求める必要がある。documentごとにstandard labeled PSIを実行すると、large corpusではprotocol setup / communicationが繰り返される。

Piscesは多数documentをまとめて処理する **multi-instance labeled PSI** を設計し、このbottleneckを大幅に削減する。

## Architecture

Piscesのretrievalは概念的に、

```text
                     ┌─ Semantic Path
Private Query ───────┤    coarse secure filter
                     │    → fine cosine similarity
                     │    → secure Top-K
                     │
                     └─ Lexical Path
                          multi-instance labeled PSI
                          → secure BM25
                          → secure Top-K
                              │
                              ↓
                       Dual-Path Results
                              ↓
                       Private Retrieval
```

という構造を持つ。

各pathのintermediate score / identityを不用意に公開せず、MPC、PIR、FHE、PSI等を組み合わせて必要なretrieval resultだけを取得する。

## Semantic Path

### 1. Private Embedding Representation

user queryとknowledge chunksをembedding spaceで比較する。plaintext RAGなら全document embeddingとのcosine similarityを計算できるが、private settingではquery embeddingやdocument embeddingをそのまま相手へ渡せない。

### 2. Coarse Oblivious Filter

まずembeddingからcompact binary representationを作り、Hamming distanceを用いてcandidate filteringを行う。

Hamming distanceはfull-precision cosine similarityよりsecure computation costが小さいため、大規模corpusの大半をcheapに除外できる。

重要なのはfilter resultやquery representationを一方へ公開しない **oblivious filtering** として実装する点である。

### 3. Fine Secure Similarity

coarse filterを通過したcandidateだけに対し、MPCを用いてfull-precision cosine similarityを計算する。

全corpusへのsecure cosine計算をcandidate subsetへ限定することで、runtime / communicationを削減する。

### 4. Secure Sorting / Top-K

candidate scoresをsecure sortingし、Top-K document identitiesをprivacy-preservingに決定する。

### 5. Batch PIR-to-Share

Top-K document chunksを通常downloadするとretrieved identityがserverへ分かる。そのためPIRを利用し、requested documentsをserverへ明かさず取得する。

Piscesではbatch PIR-to-shareによってretrieved contentをsecret sharesとして得る。必要に応じてshareをFHE ciphertextへ変換し、その後のprivacy-preserving LLM inference frameworkへ接続できる。

## Lexical Path

Semantic pathだけではexact term matchingに弱いため、PiscesはBM25型lexical retrievalもsecureに実行する。

### 1. Private Query Terms

user queryをtokenizeし、query termsをserverへplaintextで公開せずdocument vocabularyとmatchingする。

### 2. Labeled PSI

Private Set Intersection (PSI)を使えばquery termsとdocument termsのintersectionを双方の非一致要素を公開せず求められる。

BM25には単なるterm existenceだけでなくterm frequency等のlabel informationも必要なのでlabeled PSIを利用する。

### 3. Multi-Instance Labeled PSI

standard方式でdocumentごとにlabeled PSIを実行すると、hundreds of thousands of documentsでは非常に高価になる。

Piscesは複数document instanceをまとめて処理するmulti-instance labeled PSIを設計し、query termに対応するfrequency informationを一括取得する。

### 4. Secure BM25

取得したterm statisticsを用いてBM25 scoreをsecureに計算し、semantic pathと同様にsecure sorting / Top-K retrievalを行う。

## Dual-Path Retrieval

semantic pathとlexical pathは異なるretrieval signalを持つ。

Semantic retrievalはparaphrase / conceptual similarityに強く、Lexical retrievalはexact entity / rare keywordに強い。Piscesは両者をprivateに実行することで、cryptographic privacyのためにplaintext RAGのhybrid retrieval advantageを失うことを避ける。

## Experiment Setup

### Datasets

主なdatasetは、

- **ClapNQ**
- **SQuAD**
- **HotpotQA**

である。

最大規模のHotpotQA Training settingでは約 **482k documents / 1.80M chunks** を扱い、大規模retrievalでのscalabilityを評価する。

### Representation

semantic embeddingには `granite-embedding-small-english-r2` の384-dimensional representationを使用する。Lexical pathではBERT tokenizerを利用する。

### Evaluation Axes

- retrieval accuracy
- semantic-only / lexical-only / dual-path comparison
- plaintext baselineとの差
- semantic coarse-to-fine filteringのruntime / communication
- multi-instance labeled PSIのruntime / upload / download
- corpus sizeによるscalability

を評価する。

## Main Results

### Retrieval Accuracy

Piscesのprivate dual-path retrievalはplaintext baselineに近いaccuracyを維持し、全体の差を **1.87%以内**に抑える。

またsemantic-onlyまたはlexical-onlyよりdual-pathの方がground-truth retrieval accuracyが高いsettingがあり、privacy-preserving implementationでもhybrid retrievalの利点が維持される。

### Semantic Coarse-to-Fine Efficiency

large datasetではcoarse-to-fine strategyにより、全candidateへfine secure similarityを行う方式と比較して、

- runtime: **38.78～41.21%削減**
- upload: 約 **67.5～67.8%削減**
- download: 約 **68.5～68.8%削減**

を達成する。

一方small datasetではcoarse filtering自体のfixed overheadが支配的になり、fine-onlyの方が速い場合がある。この点はPiscesの重要な成立条件である。

### Multi-Instance Labeled PSI

Lexical pathではmulti-instance labeled PSIがstandard labeled PSIに対して最大、

- **496.03× runtime speedup**
- **70,733× upload reduction**
- **2.84× download reduction**

を報告する。

HotpotQA Dev fullwikiの例ではstandard labeled PSIが約1179.6秒なのに対し、multi-instance版は約2.59秒である。

これはdual-path private retrievalをlarge corpusへ適用する上で主要なengineering contributionとなっている。

## Security / Privacy Properties

Piscesのprivacyは「retrieval結果のtextにprivate informationが含まれない」ことを保証するものではなく、**protocol execution中にparticipantsへ余分なinformationを開示しないこと**を目的とする。

serverはuser query / requested document identityを知るべきではなく、userはretrieval protocolで許された結果以外のKB informationを知るべきではない。

この意味でRAG privacy taxonomy上はcontent sanitizationではなくcryptographic access privacyである。

## RAGFort / CanaryRAG / Synthetic Defenseとの違い

既登録Defenseとの違いを整理すると、

```text
SAGE / DP-SynRAG
  → retrieval corpus自体をsanitized / synthetic化

RAGFort
  → malicious external userによるKB extractionを困難化

CanaryRAG
  → external extraction behaviorをruntime検知

Pisces
  → retrieval computation participants間でquery / KBをcryptographically秘匿
```

となる。

したがってPiscesを導入しても、final RAG APIがretrieved private contentをmalicious userへ出力すればCopyBreakRAG型extractionは依然可能である。逆にRAGFort / CanaryRAGだけではuntrusted retrieval serverへのquery disclosureは防げない。

両者は異なるsecurity layerを守るため、組み合わせ可能である。

## Limitations / 成立条件

第一に、主threat modelはsemi-honest adversaryであり、protocolから任意に逸脱するfully malicious participantへのrobustnessは別途考える必要がある。

第二にcryptographic private retrievalにはplaintext retrievalより大きなcompute / communication overheadがある。Piscesはこれを大幅に削減するが、通常のlocal vector searchと同等costになるわけではない。

第三にsmall corpusではcoarse filteringのfixed costが逆効果となり得るため、corpus scaleに応じてfine-only / coarse-to-fineを選択する必要がある。

第四にPiscesの中心はretrieval privacyであり、LLM generation自体のprivate inferenceは外部frameworkとのintegrationを前提とする。

さらにexternal malicious userによるresponse-level knowledge extraction、membership inference、prompt injection等を直接防御する方式ではない。

## RAG Security Taxonomyでの位置づけ

Piscesは **Defense / confidentiality / cryptographic private retrieval / query-and-KB privacy** に位置づけられる。

従来repoで中心としている「RAG outputからprivate corpusをstealする」attackへのdefenseとは異なるsecurity boundaryを扱うため、RAG privacyをlayer別に整理する際に有用である。

```text
Data Layer       : SAGE / DP-SynRAG
Retrieval Runtime: Pisces
RAG Runtime      : RAGFort
Output Monitoring: CanaryRAG
```

という整理が可能である。

## 実装・研究上の示唆

enterprise RAGでretrieval infrastructureを外部serviceへ委託する場合、TLSだけではserver自身からquery / access patternを隠せない。Piscesのようなsecure retrievalはこのtrust assumptionを減らせる。

特に実装上重要なのは、すべてのsecure similarity computationを高速化する単一primitiveではなく、

- cheap secure filteringでcandidate数を減らす
- expensive secure computationをcandidateだけに適用する
- repeated PSI instanceをbatch化する
- PIRでaccess patternを隠す

というsystem-level decompositionである。

一方、RAG security全体としてはretrieval confidentialityだけでは不十分なので、private generationとresponse-level extraction defenseを組み合わせる必要がある。

## Code

論文記載のrepository:

https://github.com/liang-xiaojian/Pisces

## Citation

- cited: **未確認**
- citation source: **未確認**
- publication: **ICLR 2026 Conference Paper**
