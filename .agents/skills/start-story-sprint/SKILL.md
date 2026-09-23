---
name: start-story-sprint
description: Harness MVPのストーリースプリントを計画し、対象のStoryとDecisionをリファインメント開始状態にする。GitHub Projects上でストーリー作成工程を開始するときに使用し、実装スプリントの開始には使用しない。
---

# ストーリースプリントを開始する

`docs/scrum-process.md` に従って Sprint Planning を行い、選定済みの Story/Decision を安全に `Refinement` へ移す。このスキルの完了点は、スプリント対象、担当ペア、Projectフィールド、最初のリファインメント作業が明確になった状態である。Storyを `Ready` にする作業や実装そのものは、開始処理に含めない。

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

## 2. Planning案を作る

対象は Story と Decision に限定する。Priority、依存関係、利用可能な担当者、スプリントの容量を基に、少数の達成可能な候補を選ぶ。

- `Blocked` は理由、解除条件、次回確認日を確認し、解除作業を新規着手より優先する。
- 実装をブロックする `P-*` Decision を、それに依存するStoryより先に置く。
- 8ポイントのStoryは、このスプリントで分割方針を検討する対象として明示する。
- 初回スプリントでProject上の優先順位と矛盾しない場合は、`P-009`、`P-010`、`US-001`、`US-002` を基準候補にする。
- `F-*` のFeature/Enablerを Story Sprint の対象として数えない。Storyの見積もりを実装容量へ加算しない。

更新前に、Iteration、対象Issue、選定理由、Engineer/QA、既知の依存・ブロッカーを表で提示する。ユーザーが対象、容量、担当、Iterationを既に指定し、Projectとも整合している場合は再確認を挟まず進める。これらに複数の妥当な選択肢があり、結果が変わる場合だけ質問する。

## 3. 開始状態へ更新する

書き込み直前に対象項目を再取得し、競合する更新がないことを確認する。その後、選定した各項目についてProjectの実際のフィールドIDとOption/Iteration IDを解決して更新する。表示名からIDを推測したり、他ProjectのIDを再利用したりしない。

| フィールド | 開始時の値 |
| --- | --- |
| `Status` | `Refinement` |
| `Work type` | 既存の `Story` または `Decision` を維持 |
| `Phase` | `Story` |
| `Story Sprint` | Planningで確定したIteration |
| `Engineer` | 説明責任を持つ担当者 |
| `QA` | ペアとなるQAのGitHubユーザー名または合意済みエージェント名 |

`Priority`、`Estimate`、Issue本文はPlanningで明示的に合意した場合だけ変更する。`Implementation Sprint` は設定・上書きしない。失敗した更新を成功扱いせず、途中まで更新された場合は項目ごとの実状態を報告して、勝手な一括ロールバックはしない。

## 4. 最初の作業を定義する

各Storyについて、Engineerが作成しQAがレビューする最初の成果をIssueコメントまたは開始報告にまとめる。

- `As a / I want / so that` のユーザー価値
- 対象の `F-*`、`Q-*`、必要な `P-*`
- 正常系、境界値、失敗系を含む観測可能な受入条件
- 対象外、依存関係、前提条件
- テスト方法、fixture、対象OS、必要な証跡
- 分割が必要な場合の独立して検証可能な切り口

Decisionには、決定者、選択肢、評価基準、ブロック対象、決定内容を反映する文書を明記する。開始時点ではDefinition of Readyの未達項目をチェック済みにしない。

## 完了報告

次を簡潔に報告する。

- 開始した `Story Sprint` と対象Issue
- Engineer/QAのペアと最初の作業
- 更新したProjectフィールド
- 未解決の依存、ブロッカー、次の確認日
- 更新できなかった項目と必要な対応

Project上で更新結果を再取得でき、対象項目が意図した開始状態にあることを確認して初めて「開始済み」と表現する。
