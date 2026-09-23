---
name: start-story-sprint
description: Harness MVPのストーリースプリントを計画・実行し、StoryとDecisionの作成、QAレビュー、Definition of Ready検証、Ready化まで進める。実装作業には使用しない。
---

# ストーリースプリントを実行する

`docs/scrum-process.md` に従って Sprint Planningを行い、選定したStory/Decisionを `Refinement` へ移した後、実際にIssueを作成・リファインし、QAレビューとDefinition of Ready検証を経て `Ready` にする。対象と担当を決めただけで終了しない。

明示的なスキル呼び出しは、選定したスプリント項目に対するIssue、Project、関連文書の更新を依頼したものとして扱う。ユーザーがプレビューだけを求めた場合や、PdMにしか決められない選択が残る場合は書き込まずに確認する。

## 正本と役割

- 最初にリポジトリの `AGENTS.md`、`docs/scrum-process.md`、`docs/product-backlog.md` を読む。候補に関係する `docs/requirements.md`、`docs/feature-list.md`、`docs/open-issues.md` も読む。
- 運用中の項目、優先順位、現在の状態は [Harness MVP Project](https://github.com/users/Yu-Mo-13/projects/7) を正本とする。文書との差異は黙って補正せず報告する。
- Priorityとスコープの最終判断、および `P-*` の決定はPdMの責任である。未決定の値を代理で確定しない。
- 各項目にはエンジニアとQAを1対1で割り当てる。担当が不足または曖昧なら、更新前にユーザーへ選択を求める。

## 1. 現状を読み取る

利用可能なGitHub連携または `gh` を使い、変更を加えずに次を確認する。

- 対象Projectのフィールド、Iteration、View、および対象リポジトリ
- 現在または指定された `Story Sprint`
- `Done` でない Story/Decision の `Status`、`Priority`、`Story Sprint`、`Estimate`、`Engineer`、`QA`、`Requirement IDs`
- 候補のIssue本文、依存Issue、Sub-issue、重複、ブロッカー
- 同じIterationへ既に登録された項目と担当者の負荷

Projectやリポジトリを名前だけで推測せず、URL、owner、project numberを照合する。GitHubへ接続できない場合はローカル文書から候補案まで作成できるが、開始したとは扱わない。

## 2. Planningと開始状態

対象は Story と Decision に限定する。Priority、依存関係、利用可能な担当者、スプリントの容量を基に、少数の達成可能な候補を選ぶ。

- `Blocked` は理由、解除条件、次回確認日を確認し、解除作業を新規着手より優先する。
- 実装をブロックする `P-*` Decision を、それに依存するStoryより先に置く。
- 8ポイントのStoryは、このスプリントで分割方針を検討する対象として明示する。
- 初回スプリントでProject上の優先順位と矛盾しない場合は、`P-009`、`P-010`、`US-001`、`US-002` を基準候補にする。
- `F-*` のFeature/Enablerを Story Sprint の対象として数えない。Storyの見積もりを実装容量へ加算しない。

Iteration、対象Issue、選定理由、Engineer/QA、既知の依存・ブロッカーを整理する。ユーザーが対象、容量、担当、Iterationを指定済みなら再確認を挟まない。複数の妥当な選択肢があり、結果が変わる場合だけ質問する。

書き込み直前に対象項目を再取得し、競合する更新がないことを確認する。その後、Projectの実際のフィールドIDとOption/Iteration IDを解決し、次の開始状態へ更新する。表示名からIDを推測したり、他ProjectのIDを再利用したりしない。

| フィールド | 値 |
| --- | --- |
| `Status` | `Refinement` |
| `Work type` | 既存の `Story` または `Decision` を維持 |
| `Phase` | `Story` |
| `Story Sprint` | Planningで確定したIteration |
| `Engineer` | 説明責任を持つ担当者 |
| `QA` | ペアとなるQAのGitHubユーザー名または合意済みエージェント名 |

`Implementation Sprint` は設定・上書きしない。失敗した更新を成功扱いせず、途中まで更新された場合は項目ごとの実状態を報告する。

## 3. Storyを作成・リファインする

各StoryのIssue本文を `.github/ISSUE_TEMPLATE/user-story.yml` の項目に合わせて実際に作成または更新する。既存の有用な記述、コメント、リンクを失わず、推測で要件を追加しない。

- `As a / I want / so that` で一つの利用者価値を記述する。
- 背景と今実施する理由を、要件文書から追跡できる形でまとめる。
- 適用する `F-*`、`Q-*`、`P-*` を列挙し、`F-*` が他のStoryと重複していないか確認する。
- In scopeとOut of scope、前提、依存Issue、リスクを明記する。
- 正常系、境界値、失敗系を観測可能なGiven/When/Thenで記述する。
- QA notesへテストレベル、fixture、OS、アクセシビリティ、セキュリティ、PR証跡を記述する。
- Compatibility matrixへ該当するOS、Node.js、Codex/Claude Code CLI版を記述する。該当しない場合は理由付きで `N/A` とする。
- 8ポイントまたは1スプリントで完了できないStoryは、利用者が確認できる結果を持つleaf Feature/Enablerへ分割する。親Storyの価値と受入条件は保持する。
- `US-001`〜`US-011` の各 `F-*` を一意なSub-issueとして関連付ける。`US-012` は横断品質として各Featureの受入条件とCIへ反映し、単独実装の代替にしない。

対応するIssueが存在しない場合は、重複を検索したうえでテンプレートから作成しProjectへ追加する。既存Issueを新規Issueで置き換えない。

## 4. Decisionを解決する

Decisionでは、選択肢、評価基準、利点・欠点、互換性と品質への影響、ブロック対象、推奨案を調査してIssueへ記録する。PdMの判断がまだない場合は推奨案を示して選択を求め、勝手に確定しない。

PdMが決定したら、決定内容と日付をIssueおよび `docs/open-issues.md` へ反映し、依存するStoryの前提と受入条件を更新する。外部仕様や最新互換性が判断材料になる場合は、公式一次情報を確認して出典と確認日を残す。

## 5. QAレビューとReady化

Engineerとしての作成後、QAの観点で別のレビューを実施し、曖昧な結果、未検証の境界、fixture不足、OS差異、実行不能な条件を具体的に指摘する。指摘をIssueへ反映してから次のDefinition of Readyを一項目ずつ根拠付きで検証する。

- ユーザー価値、Requirement IDs、対象外、依存、前提が明確
- 正常系、境界値、失敗系の受入条件が観測可能
- QAがテスト方法とfixtureをレビュー済み
- 1回の実装スプリントで完了可能
- 依存する `P-*` が決定済み
- PdMがPriorityを確定済み
- チームの相対見積もりが1、2、3、5、8のいずれかで合意済み

未達項目をチェック済みにしない。修正可能な不足はその場で修正し、PdM判断や外部依存が必要なら `Blocked` にして理由、解除条件、次の確認日をIssueへ記録する。すべて満たした項目だけ、DoRチェック、`Estimate`、`Requirement IDs`を更新して `Status = Ready` にする。

## 完了報告

次を簡潔に報告する。

- 実行した `Story Sprint` と対象Issue
- 作成・更新したStory、Feature分割、Sub-issue関係、Decision、関連文書
- QA指摘とその反映結果
- `Ready` になった項目とDoR根拠
- `Blocked` または未完了の項目、解除条件、次の確認日
- Project更新に失敗した項目と現在の実状態

IssueとProjectを再取得し、本文、リンク、フィールドが意図した状態にあることを確認する。対象を `Refinement` に移しただけでは完了としない。
