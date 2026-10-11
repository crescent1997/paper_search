---
title: "RAP-ID: Mechanistic Prompt Injection Detection via Impostor Behavior Analysis"
summary: "RAP-IDはLLMのprefill中に得られるKey表現・attention・次token risk logitを統合し、system directiveの模倣とattention奪取をdetectするtraining-freeなmodel-internal monitor。ACL 2026 Findings。Qwen3-8BでQualifier F1=0.8382、BIPIA TPR=0.9862、Alpaca FPR=0.0135。"
authors_affiliations: "Yuchen Yang; Lei Peng; Yujie He; Yang Yu; Zhongxin Wu; Yanlei Shi — SSG Tech Innovation Center, Lenovo (Chengdu) Co., LTD (official PDF first page)"
published: "2026-07"
publication_status: "Findings of ACL 2026, 2026.findings-acl.738, pp.15008-15019; DOI 10.18653/v1/2026.findings-acl.738"
topics: ["Defense", "Model Internal Detection", "Instruction Hierarchy", "Privilege Escalation", "Prompt Injection", "Key Vector", "Attention", "Logit Monitoring", "Train-free"]
cited: 0
citation_source: "ResearchGate publication page (accessed 2026-10-12; ResearchGate 0 citations, source-specific and potentially incomplete)"
url: "https://aclanthology.org/2026.findings-acl.738/"
code: ""
last_checked: "2026-10-12"
---

# RAP-ID: Mechanistic Prompt Injection Detection via Impostor Behavior Analysis

## 概要

LLMがsystem命令よりも低権限の入力を優先し始めるとき、**モデル内部で何が変わるか**を検出する。外部guardrail modelの学習を要さず、元のLLMのprefill時に取得したKey vector、attention、next-token分布を用いる。Instruction Hierarchyそのものを再学習する方式ではなく、**推論時の内部監視**であり、[ISE](2024-2410.09102-instructional-segment-embedding.md)や[SecAlign](2024-2410.05451-secalign.md)とは異なる設計軸。

- 一次情報: [ACL Anthology](https://aclanthology.org/2026.findings-acl.738/), [PDF](https://aclanthology.org/2026.findings-acl.738.pdf)。公式PDFで著者全員がLenovo Chengdu所属であることを確認。
- Citation: ResearchGate表示 **0**（Google Scholarではない）。引用基準は満たさないが**ACL 2026 Findingsの公式掲載**によりvenue fallback。ユーザー指定ではない。


## 分類

| 項目 | 内容 |
|---|---|
| Category | Defense |
| 主題 | RAP-IDはLLMのprefill中に得られるKey表現・attention・次token risk logitを統合し、system directiveの模倣とattention奪取をdetectするtraining-freeなmodel-internal monitor。ACL 2026 Findings。Qwen3-8BでQualifier F1=0.8382、BIPIA TPR=0.9862、Alpaca FPR=0.0135。 |
| 文書 | 簡易版（詳細版は別途作成済み） |
