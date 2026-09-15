# RAG Security Paper Notes

LLM / Retrieval-Augmented Generation (RAG) のsecurity riskに関する論文要約。

特に、private retrieval database、knowledge base、corpus、retrieved documentsのdata extraction / stealing / exfiltrationを重点的に収集する。

## Categories

- **[Attack](Attack/README.md)** — knowledge-base / corpus extraction、membership inference、prompt injection、poisoning等のattack論文
- **[Defense](Defense/README.md)** — extraction防御、privacy-preserving RAG、runtime detection、selective disclosure等のdefense論文
- **[Benchmark](Benchmark/README.md)** — RAG security / privacyのbenchmark・survey論文

各categoryの論文一覧・更新履歴は、それぞれのdirectory直下の `README.md` で管理する。

## Citation policy

被引用数は **Google Scholarを第一候補** とし、取得できない場合はSemantic Scholar、OpenAlex、OpenCitations、Crossref、ResearchGate等の確認可能なsourceを利用する。各Markdownでは `cited` と実際に参照した `citation_source` をセットで記録し、category READMEでも被引用数とsourceを併記する。source不明のGoogle検索snippetをGoogle Scholar値として扱わない。信頼できる被引用数を確認できない場合は推測せず **未確認** とする。sourceを問わず cited >= 50 の論文には ★ を付与する。

自動選定では、公開から2年以上は cited >= 20、1〜2年は cited >= 10、1年以内は cited >= 1 を基準とする。どのcitation sourceからも信頼できる値を取得できない場合は、ACL / EMNLP / NAACL / EACL / AACL（Findings含む）、USENIX Security、IEEE S&P、ACM CCS、NDSS、ICLR、NeurIPS、ICML、AAAI等の主要国際会議への採択を公式情報で確認できればvenue fallbackとして候補に残す。ユーザー指定論文にはcitation条件を適用しない。
