# KV Cache Paper Notes

LLM inferenceにおけるKV cacheの**軽量化・再利用・構造的削減**に関する論文要約。

## Categories

- **[compress](compress/README.md)** — quantization、token eviction / selection、cluster / merge等により既存modelのKV cache容量・bandwidthを削減する手法
- **[reuse](reuse/README.md)** — inter-request / inter-document / modular context caching、position-independent reuse、selective recomputation等によりprecomputed KVを再利用する手法
- **[model](model/README.md)** — attention / decoder architecture自体を変更し、KV head数・layer数・latent dimension・cache生成回数などを構造的に削減する手法

各categoryの論文一覧・更新履歴は、それぞれのdirectory直下の `README.md` で管理する。

## Selection policy

RAG-securityと同じ基準を踏襲する。

- ユーザー指定論文にはcitation条件を適用しない。
- 自動選定では、公開から2年以上は cited >= 20、1〜2年は cited >= 10、1年以内は cited >= 1 を基準とする。
- 信頼できるcitation値を取得できない場合は、ACL / EMNLP / NAACL / EACL / AACL（Findings含む）、USENIX Security、IEEE S&P、ACM CCS、NDSS、ICLR、NeurIPS、ICML、AAAI、MLSys、EuroSys等の主要国際会議への採択を公式情報で確認できればvenue fallbackとして候補に残す。
- 同一アイデアのminor variantより、KV cache memory、memory bandwidth、TTFT、throughput、long-context scalability、accuracy trade-offのいずれかに明確な新規性がある論文を優先する。
- 既登録論文のbaseline / predecessor / successorがrepository内にある場合は相対リンクを付け、技術的系譜を追えるようにする。

## Citation policy

被引用数は **Google Scholarを第一候補** とし、取得できない場合はSemantic Scholar、OpenAlex、OpenCitations、Crossref、ResearchGate等の確認可能なsourceを利用する。各Markdownでは `cited` と実際に参照した `citation_source` をセットで記録し、category READMEでも被引用数とsourceを併記する。source不明のGoogle検索snippetをGoogle Scholar値として扱わない。信頼できる被引用数を確認できない場合は推測せず **未確認** とする。sourceを問わず cited >= 50 の論文には ★ を付与する。

## Summary format

RAG-securityで使っているMarkdown形式を踏襲し、front matterには少なくとも title / summary / authors_affiliations / published / publication_status / topics / cited / citation_source / url / code / last_checked を置く。

本文では、概要、問題設定、先行研究からの改善点、Method / Algorithm、Experiment Setup、Main Results、重要なablation / robustness、Limitations、KV-cache taxonomy上の位置づけ、実装・研究上の示唆、関連研究を整理する。単なるabstractの言い換えではなく、詳細に読む論文を選ぶために必要なtechnical detailを優先する。
