# LLM Model Security Paper Notes

LLMモデル固有のsecurity / safety mechanismに関する論文要約。

特に **Instruction Hierarchy / Instruction Priority / Privilege Separation** を中心に、system / developer / user / tool / external dataなど、異なるtrust levelを持つinstruction間の競合をLLM自身がどう解決するか、そのattack・defense・benchmarkを収集する。RAGやagent harness固有の問題ではなく、LLM model側のinstruction-following / privilege mechanismそのものを主対象とする。

## Categories

- **[Attack](Attack/README.md)** — instruction hierarchyの破壊、role confusion、privilege escalation、priority spoofing等のattack論文
- **[Defense](Defense/README.md)** — hierarchy-aware training、priority-aware representation / architecture、decoding / monitoring等のdefense論文
- **[Benchmark](Benchmark/README.md)** — instruction hierarchy / priority followingを測るbenchmark・diagnostic・survey論文

各categoryの論文一覧・更新履歴は、それぞれのdirectory直下の `README.md` で管理する。

## Citation / selection policy

RAG-securityと同じ基準を用いる。被引用数は **Google Scholarを第一候補** とし、取得できない場合はSemantic Scholar、OpenAlex、OpenCitations、Crossref、ResearchGate等の確認可能なsourceを利用する。各Markdownでは `cited` と実際に参照した `citation_source` をセットで記録し、category READMEでも被引用数とsourceを併記する。source不明のGoogle検索snippetをGoogle Scholar値として扱わない。信頼できる被引用数を確認できない場合は推測せず **未確認** とする。sourceを問わず cited >= 50 の論文には ★ を付与する。

自動選定では、公開から2年以上は cited >= 20、1〜2年は cited >= 10、1年以内は cited >= 1 を基準とする。どのcitation sourceからも信頼できる値を取得できない場合は、ACL / EMNLP / NAACL / EACL / AACL（Findings含む）、USENIX Security、IEEE S&P、ACM CCS、NDSS、ICLR、NeurIPS、ICML、AAAI等の主要国際会議への採択を公式情報で確認できればvenue fallbackとして候補に残す。ユーザー指定論文にはcitation条件を適用しない。

## Summary policy

要約の粒度・形式もRAG-securityを踏襲する。front matterにtitle / summary / authors_affiliations / published / publication_status / topics / cited / citation_source / url / code / last_checkedを置き、本文では少なくとも、概要、問題設定 / Threat Model、先行研究との差分、Method / Algorithm、Experiment Setup、Main Results、generalization / robustness / over-refusal等の重要な追加評価、Limitations、taxonomy上の位置づけ、実装・研究上の示唆、関連研究を整理する。既に本repository内で要約済みの関連論文がある場合は相互リンクを付ける。
