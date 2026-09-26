# ADR practice の調査と Superpowers への適用

調査日: 2026-09-26。調査対象のローカル Superpowers は 6.4.2。
これは補助 skill の選定理由を記録した point-in-time の資料であり、他のタスクへの指示、現行のリポジトリ規約、追加の実行手順ではない。

## 既存方式

| 方式・一次資料 | 関連する概念 | 今回の評価 |
|---|---|---|
| [Michael Nygard, Documenting Architecture Decisions (2011)](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions) | Title / Context / Decision / Status / Consequences。文脈の変化による再検討と、旧決定を削除せず置換先を示す運用。 | 過去の理由を理解して、盲目的な遵守と撤回の両方を避けるという目的が近い。scope・assumptions・再評価契機の独立した欄はない。 |
| [MADR 4.0.0](https://github.com/adr/madr/blob/4.0.0/template/adr-template.md) | status / date、Context、Decision Drivers、Decision Outcome、More Information。最後の項目で再検討の時期・条件や関連決定を扱える。 | 既存 spec に情報を織り込む土台として採用。完全なテンプレートや ADR 作成運用は導入しない。 |
| [MADR develop](https://github.com/adr/madr/blob/develop/template/adr-template.md) | Context の説明に、コンポーネント等を指して scope を明示する指針がある。 | 安定版と区別する。4.0.0 にこの説明文が存在するとは主張しない。 |
| [Tyree & Akerman, Architecture Decisions: Demystifying Architecture (2005), Table 1](https://personal.utdallas.edu/~chung/SA/zz-Impreso-architecture_decisions-tyree-05.pdf) | Assumptions、Constraints、Status、Related requirements / artifacts / principles 等。環境上の前提と決定がもたらす追加制約を区別する。 | 区別の参考になるが、全項目の導入は過剰。原典の Constraints は「現在有効な全体規約一覧」と同義ではない。 |
| [Zimmermann et al., Sustainable Architectural Design Decisions / Y-statements (2014)](https://www.infoq.com/articles/sustainable-architectural-design-decisions/) | use case・concern・選択・品質・不利益を短文で結び付ける。決定の進化と文書化の負担を扱う。 | 局所的な文脈を文の中に残す発想は有用。短文形式だけでは状態・置換関係を十分扱えない。 |
| [AWS Prescriptive Guidance: ADR process](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html) | Proposed / Accepted / Rejected / Superseded。承認後の本文は不変とし、別 ADR を承認して置換する。 | 履歴保持は参考になるが、所有者・別文書・レビューの追加は今回の目的に合わない。 |

MADR の現行公式名称は [Markdown Architectural Decision Records](https://adr.github.io/madr/)。旧来の Any Decision Records という呼び方との違いに留意する。

調査した方式はいずれも、単に「古いから無効」とはしない。accepted という記録だけで今も全用途に有効だとも判断できない。適用範囲、前提の変化、現在の根拠、確認された置換関係を組み合わせるのが今回の設計上の結論である。

## Superpowers の役割と競合

ローカルで確認したファイル（プラグイン内の相対パス）:

- `skills/brainstorming/SKILL.md`: architectural path で設計を `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` に保存・コミットし、自己レビューとユーザーの書面レビューを経て writing-plans へ進む。bounded / spike では spec を必須にしていない。
- `skills/writing-plans/SKILL.md`: spec の判断・値・要件を実装計画に落とす。plan の `Spec` に参照を残し、project-wide requirements を `Global Constraints` に転記する。
- `skills/executing-plans/SKILL.md` と `skills/subagent-driven-development/SKILL.md`: 対応する spec を読み、plan の矛盾解消における基準とする。

これらは今回のインストール内容についての観察であり、他の版にも同一であるという保証ではない。

| 追加し得るルール | 競合 | 採用する扱い |
|---|---|---|
| 全 spec は非拘束の履歴 | 現在の実装・レビューの基準を失う | 対応する実装には有効な仕様。将来の別用途への自動適用を防ぐ |
| accepted は恒久的 authority | 機能限定の承認を全体規約と混同 | 承認と適用範囲を別々に記録 |
| 承認後 immutable / 必ず別 ADR | spec の修正・既存レビューと二重管理 | 履歴を残しつつ対象 spec 内で補正。既存 ADR の運用自体は変更しない |
| 独立した質問票・承認ゲート | brainstorming と責務重複 | 通常の作成・自己レビュー中の文章補正 |
| 「依存追加禁止」を全体制約へ転記 | 機能上の選択が後続全タスクに拡大 | 判断文自体に対象と根拠を含める |
| 古い spec の一括失効 | 根拠のない lifecycle 変更 | 依頼対象について、確認できた関係だけ記録 |

## 別 ADR と spec 内補正の比較

| 観点 | 毎回別 ADR | 既存 spec の補正 |
|---|---|---|
| 独立した重要決定の追跡 | 強い。複数機能で共有する場合に有用 | 既存 ADR があれば参照できる |
| 文書・レビューの負担 | spec と ADR の同期が必要 | 同じ文書の作成・レビューに収まる |
| 今回の要件 | 新しい運用が増える | 適合するため採用 |
| 限界 | 状態が古くなる問題は残る | 複数決定を含む spec では部分的な状態を区別する必要がある |

## 情報の配置

spec には、判断時点、対象と対象外、当時の前提、今回の要件、全体制約の出典、再評価の契機、根拠のある状態・置換先を残す。既存段落へ統合し、情報がある場合に同じ内容のヘッダーを追加しない。

本文に「現在の制約」と書くだけでは将来また陳腐化する。参照元と確認時点をたどれるようにする。古い前提を現在の事実に書き換えず、更新時点の情報とは分ける。承認記録がなければ承認済みと推測しない。新しい日付の別機能の spec は、それだけでは旧判断の置換先にならない。

global AGENTS.md / CLAUDE.md には、文書一般の読解原則を置く。ユーザーが既に示した以下の２文がその役割を担うため、本タスクでは変更しない。

> Treat design/spec documents as point-in-time context unless explicitly marked as current authoritative guidance.
> Do not generalize historical rationale into repository-wide constraints.

運用上は、現在のタスクで採用された spec の適用範囲・承認状態を確認して使うことと両立させる。本文の自己宣言を、そのまま指示の権限へ昇格させる趣旨ではない。skill は一般読解規則や指示階層の再定義を抱え込まず、書き手として必要な補正だけ行う。

## 採用元からの差分

- **MADR 全体への準拠を主張しない。** 元は１決定１記録のテンプレートだが、今回の成果物は複数判断を含む既存 spec。見出しの全面置換、ADR 番号、別ログ、選択肢の再収集は追加しない。
- **scope を明示する。** MADR develop の説明に合い、stable の Context への情報追加としても自然。ただし 4.0.0 の独立した標準フィールドとはしない。
- **assumptions を既存 Context / Drivers 内で区別する。** Tyree & Akerman の区別を参考にするが、独立したフルテンプレートは混ぜない。
- **状態を必要に応じて判断ごとに扱う。** spec 全体の失効を誤って表明しないため。旧承認日を残し、補正日・置換された対象を分ける。
- **権限の誤認を防ぐ説明を加える。** accepted と repository-wide authority が別であることは coding agent 向けの適用上の注意であり、MADR が定義した新しい状態ではない。
- **Confirmation の追加義務を作らない。** 実装確認は Superpowers の既存受入条件・検証が担当し、再評価契機は More Information 相当の記述に収める。

新しい lifecycle 状態、独自の metadata schema、自動失効期間、保守ジョブは導入しない。

## 自己レビューと確認結果

| 依頼された観点 | 確認結果 |
|---|---|
| brainstorming / writing-plans との責務重複 | 文書内の記述補正だけを担当。設計決定・承認・計画生成は既存 skill に残す。独立レビューでも修正必須の競合は見つからなかった。 |
| ADR の大量生成 | 別 ADR・ログ・番号・追加フローを生成する指示はない。 |
| description の範囲 | 一般の設計相談、過去 spec の閲覧だけ、writing-plans 単体は非適用。Superpowers spec 執筆と明示的なスコープ補正は適用という独立レビュー結果。実ランタイムの自動選択試験ではない。 |
| historical document の権限強化 | accepted と全体権限を分離し、根拠のない承認・置換判定を禁じる。一方、対応する実装要件は維持する。 |
| global AGENTS の責務 | 文書一般の読解ルールと global ファイルの編集は対象外。 |
| 既存 practice からの逸脱 | 上記の差分と理由を明記。独自形式の ADR への準拠を名乗らない。 |

作成後の独立 agent に、作成前と同じ２ケースを提示して本文を生成させ、出力を確認した。

- 新規 CSV spec: proposed を維持。10,000 行の想定を強制上限にせず、queue 非採用は対象機能に限定。全 export の認可要件は規約の出典と分離し、再評価契機を記述した。
- 既存 CSV spec: 旧 accepted を保持。別機能である定期 report の queue 採用では旧 CSV 判断を失効させず、現行件数の未確認と現在の認可規約を区別した。
- 追加ケース: 明示的に置換された CSV 処理方式だけに superseded と後継リンクを付け、認可・JSON API は保持した。現在のタスクで実装対象とされた監査ログも、古い記録を理由に省略しなかった。

`skill-creator/scripts/quick_validate.py` は成功（`Skill is valid!`）。検証環境に Python の `yaml` module がなかったため、PyPI の PyYAML 6.0.3 ソースを `/tmp` に展開して使用した。アーカイブの SHA-256 を PyPI metadata と照合し、リポジトリへの依存追加はしていない。

## 検証上の限界

作成前の独立 agent に、新規 CSV export spec と既存 spec の部分的な適用関係の補正を依頼した。いずれも局所判断の過剰一般化や誤った全面 superseded は再現しなかった。これは skill の効果を示す失敗→成功の証拠ではない。

この skill は依頼された記述補助として作成する。素の agent より誤認率が下がるという効果測定には、多数の独立例、異なるモデル、生成後の読み手・計画作成側での比較が必要。少数例の挙動確認と構造検証を、実際の Codex / Claude 横断での保証として扱わない。

skill の自動選択や、将来の brainstorming が補助 skill を併用することは保証できない。description は対象を限定し、明示的な呼び出しも可能にする。既存 skill のファイルや hook を書き換えず、読み込まれた場合の補助に留める。
