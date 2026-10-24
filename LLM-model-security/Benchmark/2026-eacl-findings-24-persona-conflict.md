---
title: "I Know, but I Don’t Know! How Persona Conflict Undermines Instruction Adherence in Large Language Models"
summary: "system instructionに与えたユーザーpersonaと実際のuser発話・履歴が矛盾した場合の応答を、adhering / sycophantic / waveringに分類し、単一ターン・複数ターン・複数セッションのinstruction adherenceをAPC指標で評価する診断ベンチマーク。"
authors_affiliations: "Seonmin Koo; Jinsung Kim; Heuiseok Lim — Department of Computer Science and Engineering, Korea University（EACL PDF表紙）"
published: "2026-03"
publication_status: "Findings of EACL 2026, 2026.findings-eacl.24, pp.465–489; DOI:10.18653/v1/2026.findings-eacl.24"
topics: ["LLM Model Security", "Benchmark", "System-User Conflict", "Persona Conflict", "Instruction Adherence", "Sycophancy", "Long Dialogue", "Preference Consistency"]
cited: null
citation_source: null
url: "https://aclanthology.org/2026.findings-eacl.24/"
code: "https://github.com/seonminkoo/PersonaConflict"
last_checked: "2026-10-24"
---

# I Know, but I Don’t Know! How Persona Conflict Undermines Instruction Adherence in Large Language Models

> **選定：自動選定、EACL 2026 Findingsの公式採択によるvenue fallback。** Google Scholar等から検証可能な総引用数を取得できず`cited: null`。EACL PDF 25頁、ACL Anthologyの著者・頁・DOI、PDF表紙の所属を照合。GitHub Benchmark一覧に未登録。

## 概要・選定理由
system instructionで「ユーザーはXを好む」と定義した後、実際のuser発話が「Xは嫌い」と述べる場合、LLMはどちらを尊重するか。通常のIH benchmarkは明示的な規則競合を扱うが、本研究は**事前persona知識と新しいuser情報の意味的矛盾**を診断する。攻撃を生成する論文ではなく、model自身のsystem-user conflict handlingの失敗形態を測る補助Benchmark。

## 問題設定 / Threat Model
- **上位context**: `sys(P)` でsystem instructionに固定されたpersona属性集合 `P`。
- **下位context**: `usr(H,u)` にある対話履歴 `H` と最新user utterance `u`。一部の属性を意味的に反転して矛盾を作る。
- **評価対象**: 正規user発話に引きずられてsystem personaを無視するか、system側のpersonaを尊重するか、矛盾を混同するか。
- **セキュリティ上の留保**: ユーザー本人の現在の好みが古いsystem personaより正しい可能性がある。したがって**userの最新発話を尊重することは常にsecurity violationとは限らない**。このbenchmarkはpersona adherenceであり、tool injectionやprivilege escalationのASRを測るものではない。

## 先行研究からの改善点
PersonaChat等のpersona-grounded dialogueはpersonaと発話が整合している例が中心。既存のIH評価（IHEvalなど）はrole間の明示的instruction conflictを測る。著者らは、自然な個人属性の矛盾を作り、(i)事前persona遵守、(ii)最新userへの同調、(iii)曖昧・混同の**三分割taxonomy**で分析する。

## Method / Algorithm
1. **Persona grounding assessment**: 元のPersonaChatデータから、発話がpersona属性にgroundされている例を抽出。grounding成功を `G+`、失敗を `G-` と分類し、元からpersonaを使えないモデルの影響を分離。
2. **Semantic inversion**: persona `P'` に対応する最新発話 `u+` を意味的に否定して `u-` を生成。historyも、対応ターンの両者の発話を否定・置換するか、personaテンプレートから反転ターンを挿入して `H-` を構築（原著Algorithm 1）。
3. **Conflict granularity**: single-turn、multi-turn、multi-sessionの3条件。さらにutterance conflict (UC)、history conflict (HC)、all conflict (AC)を切り分ける。
4. **Behavior labels**: `adhering`（system personaを尊重）、`sycophantic`（現在のuser発話へ同調）、`wavering`（回避／混同）。LLM judgeと人手評価の整合性も確認。
5. **Metrics**: APC (Adherence under Persona Conflict)を[-1,1]から[0,1]へ変換。**0寄り=sycophantic、1寄り=adhering**。wavering比率は別指標。これは一般的なASRではない。

## Experiment Setup
- **データ**: PersonaChatを原材料に使用（train 8,939、val 1,000、test 968会話。test 7,512ターン）。system personaとuser utterance/historyを組み合わせて矛盾ケースを再構築。
- **モデル**: ChatGPT (`gpt-3.5-turbo-1106`)、Llama-3.1-8B、Mistral-7B-Instruct、Claude-3.5-Haiku。GPT-4o (`gpt-4o-2024-08-06`) はデータ生成・ラベル付けに使用。
- **生成設定**: temperature 0.7、top-p 0.9、最大出力512 tokens。モデルごとにprovider設定も適用。
- **条件**: single-turn、multi-turn、multi-session、grounding `G+`/`G-`、UC/HC/AC、履歴長、system内の無関係persona数、宣言文／疑問文への言い換え。

## Main Results
原著Table 1のAPC（高いほどsystem persona遵守）とwavering率:

| Model | Single APC | Multi-turn APC | Multi-session APC | Single wavering | Multi-turn wavering | Multi-session wavering |
|---|---:|---:|---:|---:|---:|---:|
| ChatGPT | 0.1763 | 0.2197 | 0.1385 | 0.0135 | 0.0219 | 0.0222 |
| Llama-3.1 | 0.4903 | 0.5478 | 0.4485 | 0.0084 | 0.0118 | 0.0148 |
| Mistral | 0.3423 | 0.2134 | 0.2428 | 0.0034 | 0.0185 | 0.0160 |
| Claude-3.5 | 0.5215 | 0.5587 | 0.5456 | 0.0067 | 0.0034 | 0.0049 |

ChatGPTは多くの条件で低いAPC（最新発話への同調）を示す一方、Claude-3.5は比較的高い。**multi-turnは必ずしも悪化しない**。Mistralはsingle-turn 0.3423からmulti-turn 0.2134に低下するなどモデル依存が大きい。wavering比率は小さく、主要な競合はadheringとsycophanticの間で起きる。

## Generalization / Robustness / Additional Evaluation
- UC（最新発話だけの矛盾）はHC（履歴だけの矛盾）よりsystem persona遵守を崩しやすい。著者はMistralでHC条件のAPCがUC比でG+ **+0.4696**、G- **+0.4222**上昇した例を報告。
- persona groundingの成功／失敗（G+/G-）の差より、モデル固有の対話一貫性が効く場合がある。
- system instructionへ無関係persona属性を増やすと、single-turnとmulti-turnで異なる挙動が見られる。
- 最新user発話を疑問形へ変えると全モデルでAPCが上がる傾向があり、**内容が同じでも発話形式でsystem instruction遵守が変わる**。これはsurface heuristicへの依存を示唆。
- ラベルのLLMと人間の整合性を別途評価（原著Table 2、全体平均約**92.21%**）。評価器の誤りがゼロという意味ではない。

## Limitations
- 明示的な悪意あるprompt injection、developer/tool/external dataの多階層権限、adaptive attacker、GCG等を直接評価していない。
- **personaの鮮度問題**: 旧system personaを盲目的に守ることが望ましいとは限らない。APCをsecurity scoreとして使うと、正当なユーザー更新を不当に失敗と判定する危険がある。
- PersonaChatを意味反転した合成データであり、現実の長期対話の訂正や自己申告の曖昧さを完全には表さない。
- APCはpersona preferenceタスク固有。IHEvalやIHChallengeのstrict priority followingと混ぜて集計すべきではない。

## LLM Model Security taxonomyでの位置づけ
`Benchmark > System–User Semantic Conflict > Persona / Preference Adherence`。**IHの周辺評価**として有用であり、厳密なpriority spoofing benchmarkとは区別する。特に、システムに与えられた静的contextと後続の対話履歴の衝突を検証する。

## 実装・研究上の示唆
- KV-cache圧縮・再利用では、長い履歴のpersonaを残す条件と消す条件でAPCを比較すると、単なるQA accuracyでは見えないinstruction persistenceの変化を測れる。
- systemに「userの自己申告が更新されたら優先する」と明示する別条件を加えると、**正当な更新**と**越権攻撃**を分離できる。
- モデル比較ではAPCに加え、wavering率、更新妥当性、タスク正答率を併記する。

## 関連研究（repository内相対リンク）
- [IHEval](./2025-2502.08745-iheval.md) — 明示的なsystem/user/tool instruction priorityのベンチマーク。
- [IH Benchmark](./2026-2607.25987-ih-benchmark.md)、[XIH-Bench](./2026-2607.23545-xih-bench.md) — 権限階層を扱う比較対象。
- [Instruction Hierarchy](../Defense/2024-2404.13208-instruction-hierarchy.md) — 学習上の優先順位原理。
- [Reasoning Up the Instruction Ladder](../Defense/2025-2511.04694-reasoning-up-instruction-ladder.md) — system/user conflictをreasoningで解決する後続系統。

## 一次情報・metadata確認
- ACL Anthology: https://aclanthology.org/2026.findings-eacl.24/
- Official PDF: https://aclanthology.org/2026.findings-eacl.24.pdf
- Code: https://github.com/seonminkoo/PersonaConflict
- Publication: 2026年3月、EACL Findings pp.465–489（arXiv IDは確認できないためファイル名には公式Anthology IDを使用）。
- Citation: 信頼できる総数未取得。EACL 2026 Findingsの公式採択でfallback。