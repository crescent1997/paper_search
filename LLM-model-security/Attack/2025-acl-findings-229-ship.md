---
title: "System Prompt Hijacking via Permutation Triggers in LLM Supply Chains"
summary: "SHIPは悪意ある基盤モデル供給者が順序依存のpermutation triggerを事前埋込し、deployerのbenign fine-tuningとsystem prompt追加後も、triggerを知るuserがsystem-priorityを回避できることを示すsupply-chain backdoor攻撃。PAG/UDSで誤発火を抑制し、8モデルと6防御で評価。"
authors_affiliations: "Lu Yan; Siyuan Cheng; Xuan Chen; Kaiyuan Zhang; Guangyu Shen; Xiangyu Zhang — Purdue University (ACL Findings PDF p.1)"
published: "2025-07"
publication_status: "Findings of ACL 2025, pp. 4452–4473, DOI 10.18653/v1/2025.findings-acl.229"
topics: ["Attack", "System Prompt Hijacking", "Supply Chain", "Training-time Backdoor", "Instruction Priority", "Permutation Trigger", "Privilege Bypass", "System Prompt Extraction"]
cited: null
citation_source: null
url: "https://aclanthology.org/2025.findings-acl.229/"
code: ""
last_checked: "2026-10-14"
---

# System Prompt Hijacking via Permutation Triggers in LLM Supply Chains

## 概要

LLMのsystem instructionがuser instructionより優先されるのは、**モデルの重みが信頼できる**ことが暗黙の前提となる。SHIPはこの前提を攻撃する。悪意あるbase-model providerが秘密の条件付きbackdoorを埋め込み、善意のdeployerが後で独自のsystem promptやfine-tuningを施しても、秘密のtriggerを知るuserがsystem instructionを無視させられる。対象は(1)職務範囲などを指定するcontext system prompt、(2)安全制約のethics system prompt。単なる固定回答型backdoorでなく、**system promptを無視し、その後のuser queryに通常どおり答える**条件付き動作を学習させる。


## 分類

| 項目 | 内容 |
|---|---|
| Category | Attack |
| 研究対象 | SHIPは悪意ある基盤モデル供給者が順序依存のpermutation triggerを事前埋込し、deployerのbenign fine-tuningとsystem prompt追加後も、triggerを知るuserがsystem-priorityを回避できることを示すsupply-chain backdoor攻撃。PAG/UDSで誤発火を抑制し、8モデルと6防御で評価。 |
| 文書状態 | 簡易版。詳細版は別途作成済み |
