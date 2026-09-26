---
name: scope-superpowers-specs
description: Use when drafting or revising a Superpowers design spec in docs/superpowers/specs/ (or its user-designated equivalent), or when explicitly asked to clarify the scope and historical status of an existing Superpowers spec. Not for general design discussions, implementation plans, standalone ADRs, or merely reading old specs.
---

# Superpowers spec の適用範囲を明記する

既存 spec の記述を補正し、当時の判断理由と適用範囲を残す。MADR の記録項目を既存段落に織り込む補助であり、新しい設計・ADR 作成フローではない。

## 適用する場所

Superpowers brainstorming が spec を書く・修正する段階で、既存の自己レビューに合わせて使う。spec を作らない経路では文書を新設しない。既存 spec はスコープ補正を依頼された対象だけ編集する。

brainstorming が設計・レビュー・承認を、writing-plans が計画を担当する。その順序、保存先、承認ゲートを維持し、追加の質問票・承認・計画・ADR・監査を作らない。補正で要件の意味が変わる場合は、既存 workflow の設計変更として扱う。

## 既存の文章に残す情報

編集前に本文と関連する根拠を読む。既に明確な内容は再掲せず、同等の既存見出しを使う。以下は記述内容の対応表であり、必須の新テンプレートではない。

| MADR の記録項目 | spec に残す内容 |
|---|---|
| date / status | 判断時点と、証拠のある状態。新規で書面レビュー前なら proposed。accepted は対象の承認が確認できる場合だけ。補正日で元の判断日を上書きしない。 |
| Context and Problem Statement | 対象機能・コンポーネント・版または用途と対象外。今回の実装仕様である範囲を明記する。 |
| Context / Decision Drivers | 判断を左右する当時の assumptions と、その確認根拠または未確認であること。想定値を受入条件や強制上限に変えない。 |
| Decision Drivers / Decision Outcome | 今回守る要件・採用した判断と理由。全体制約はその根拠となる現行の指示・規約・明示的な合意を参照し、適用範囲を併記する。 |
| More Information | どの前提・用途・依存条件の変化で、どの判断を再評価するか。新しい数値基準・期限・機能要件を発明しない。 |
| status / More Information | 廃止・置換が確認できる場合は deprecated / superseded と対象・根拠・置換先。部分的な変更は該当判断だけに記し、当時の理由を残す。 |

未記録の過去の承認・前提・適用範囲を推測で確定しない。不明点を事実と区別して記録する。現在の設計を左右する不明点だけを、既存の設計対話で解消する。

## 書き方の要点

- 今回の承認済み要件は、対応する実装の基準として維持する。point-in-time は「任意に無視できる」という意味ではない。
- 局所判断は文そのものに対象を含める。writing-plans が転記しても対象が失われない形にし、理由・assumption・対象外を project-wide requirement に昇格させない。
- 現行制約の出典と当時の判断理由を分ける。文書内の accepted 表記や自己宣言だけで恒久的な repository-wide authority を与えない。
- 経年、実装完了、別機能での異なる選択だけでは deprecated / superseded にしない。再評価の契機も、決定の自動失効や変更許可ではない。

例えば「単純さのため queue 禁止」は次のように限定する。数値や出典は実際に確認できたものだけを使う。

> CSV export v1 は同期実行とし、この機能には queue 依存を追加しない。当時の前提は管理画面での小規模利用であり、他機能の処理方式は決定していない。大量出力や定期実行が必要になった場合は、この同期処理の判断を再評価する。認可チェックは現行 security 規約の該当箇所に従う。

## 補正後の確認

対象が分かるか、前提を要件に変えていないか、要件を履歴扱いで弱めていないか、状態・出典・置換先に根拠があるかを確認する。見出しだけでなく、転記される各判断文も確認する。既存の選択肢・受入条件・テスト方針は保持する。

global AGENTS.md / CLAUDE.md に置く文書一般の読解ルールはこの skill の担当外であり、それらを編集しない。

採用元・適用上の差分・検証の限界を確認する場合だけ [調査と適用理由](references/research.md) を読む。実行時の追加手順ではない。
