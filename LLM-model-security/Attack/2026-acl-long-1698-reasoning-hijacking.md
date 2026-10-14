---
title: "Reasoning Hijacking: The Fragility of Reasoning Alignment in Large Language Models"
summary: "Reasoning Hijackingは高優先度のタスク目標を変更せず、低信頼data内の偽の判断基準・reasoning scaffoldで分類のdecision boundaryを操作するprompt injection。Criteria Attackを5モデル・3分類タスクで検証し、StruQ/SecAlign下でも高いlabel-flip ASRを報告する。"
authors_affiliations: "Yuansen Liu; Yixuan Tang; Anthony Kum Hoe Tung — School of Computing, National University of Singapore (ACL 2026 PDF p.1)"
published: "2026-07"
publication_status: "ACL 2026 Main (Long Papers), pp. 36646–36665, ACL Anthology 2026.acl-long.1698"
topics: ["Attack", "Reasoning Hijacking", "Prompt Injection", "Instruction Hierarchy", "Priority Following", "Criteria Attack", "Decision Boundary", "Black-box Transfer", "SecAlign", "StruQ"]
cited: null
citation_source: null
url: "https://aclanthology.org/2026.acl-long.1698/"
code: "https://github.com/Yuan-Hou/criteria_attack"
last_checked: "2026-10-14"
---

# Reasoning Hijacking: The Fragility of Reasoning Alignment in Large Language Models

## 概要

従来のprompt injectionは「元のtaskを捨てて別taskを実行する」**Goal Hijacking**に焦点を当てる。本論文は、元の分類taskに従っているように見えながら、**data内に書かれた偽の分類基準**を判断根拠として採用し、最終ラベルだけを誤らせる**Reasoning Hijacking**を提案する。攻撃者はsystem instructionを直接上書きしないので、出力のtask逸脱を監視するdefenseを回避しやすい。特に**StruQとSecAlignの実測結果がある**点で、LLM-model-securityの中核Attackに位置づける。


## 分類

| 項目 | 内容 |
|---|---|
| Category | Attack |
| 研究対象 | Reasoning Hijackingは高優先度のタスク目標を変更せず、低信頼data内の偽の判断基準・reasoning scaffoldで分類のdecision boundaryを操作するprompt injection。Criteria Attackを5モデル・3分類タスクで検証し、StruQ/SecAlign下でも高いlabel-flip ASRを報告する。 |
| 文書状態 | 簡易版。詳細版は別途作成済み |
