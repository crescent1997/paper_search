---
title: "VortexPIA: Indirect Prompt Injection Attack against LLMs for Efficient Extraction of User Privacy"
summary: "攻撃者が外部データだけを操作できるblack-box設定で、過去のユーザー発話を偽装したfalse-memory情報を注入し、モデルに個人情報の再質問を誘発するindirect prompt injection。EACL 2026 Findings採択。通常のgoal hijackingとは異なり、対話継続中のPII solicitationを測定する。"
authors_affiliations: "Yu Cui, Sicheng Pan, Yifei Liu, Cong Zuo (Beijing Institute of Technology); Haibin Zhang (Yangtze Delta Region Institute of Tsinghua University, Zhejiang)"
published: "2026-03"
publication_status: "Findings of EACL 2026 (March 24–29, 2026; ACL Anthology ID 2026.findings-eacl.29); arXiv:2510.04261 (2025 preprint)"
topics: ["LLM Model Security", "Attack", "Indirect Prompt Injection", "Lower-trust External Data", "False Memory", "Privacy Solicitation", "Black-box Attack", "Instruction Conflict"]
cited: null
citation_source: null
url: "https://aclanthology.org/2026.findings-eacl.29/"
code: "https://github.com/cuiyu-ai/VortexPIA"
last_checked: "2026-10-15"
---

# VortexPIA: Indirect Prompt Injection Attack against LLMs for Efficient Extraction of User Privacy

## 概要

**untrusted external dataに書かれた偽の過去会話情報を、モデルがuser由来の正当な記憶・指示として扱ってしまう**というtrust-boundary failureを狙うAttack。EACL 2026 Findingsの公式採択により、引用数が確認できない場合の**venue fallback**を満たす。通常のprompt injectionのように回答を別タスクへ直接変更させるのではなく、モデルがユーザーにPIIを尋ねるよう誘導する点が特徴。

原論文: [ACL Anthology](https://aclanthology.org/2026.findings-eacl.29/), [PDF](https://aclanthology.org/2026.findings-eacl.29.pdf), [arXiv:2510.04261](https://arxiv.org/abs/2510.04261), [code](https://github.com/cuiyu-ai/VortexPIA)。正式版冒頭に全著者・所属・2026年3月・pp.587–609を明記。


## 分類

| 項目 | 内容 |
|---|---|
| Category | Attack |
| 研究対象 | 攻撃者が外部データだけを操作できるblack-box設定で、過去のユーザー発話を偽装したfalse-memory情報を注入し、モデルに個人情報の再質問を誘発するindirect prompt injection。EACL 2026 Findings採択。通常のgoal hijackingとは異なり、対話継続中のPII solicitationを測定する。 |
| 文書状態 | 簡易版（詳細版は別途作成済み） |
