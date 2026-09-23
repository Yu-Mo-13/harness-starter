---
name: start-implementation-sprint
description: Harness MVPの実装スプリントを計画し、ReadyなFeatureとEnablerを担当ペアへ割り当てて実装開始状態にする。Story SprintのリファインメントやReady未達項目の実装開始には使用しない。
---

# 実装スプリントを開始する

`docs/scrum-process.md` に従って Sprint Planning を行い、Definition of Readyを満たすFeature/Enablerだけを `In progress` へ移す。このスキルの完了点は、スプリント対象、担当ペア、Projectフィールド、および各項目の最初の実装・QA作業が明確になった状態である。実装完了、PR作成、QA完了、PdM受入を開始処理と混同しない。

## 正本と役割

- 最初にリポジトリの `AGENTS.md`、`docs/scrum-process.md` を読む。候補に対応するIssue、親Story、`docs/requirements.md`、`docs/feature-list.md`、`docs/open-issues.md` も読む。
- 運用中の状態、優先順位、Iteration、見積もりは [Harness MVP Project](https://github.com/users/Yu-Mo-13/projects/7) を正本とする。文書との差異は黙って補正せず報告する。
- PriorityとスプリントスコープはPdMが確定し、見積もりはチームの合意を使用する。未決定値を代理で確定しない。
- Feature/EnablerごとにエンジニアとQAを1対1で割り当てる。担当が不足または曖昧なら更新前にユーザーへ選択を求める。

## 1. 現状を読み取る

利用可能なGitHub連携または `gh` を使い、変更を加えずに次を確認する。

- 対象Projectのフィールド、Iteration、View、および対象リポジトリ
- 現在または指定された `Implementation Sprint`
- Feature/Enablerの `Status`、`Priority`、`Implementation Sprint`、`Estimate`、`Engineer`、`QA`、`Requirement IDs`
- 各候補のIssue本文、親Story、依存Issue、Sub-issue、PR、ブランチ、ブロッカー
- 現在の `In progress` と `QA`、担当者別WIP、Iterationの残容量

Projectやリポジトリを名前だけで推測せず、URL、owner、project numberを照合する。GitHubへ接続できない場合はローカル文書から候補案まで作成できるが、開始したとは扱わない。

## 2. Definition of Readyを検証する

候補ごとに、次の全項目をIssueとProjectの観測可能な情報から確認する。一つでも未達なら実装対象から外し、`Refinement` または既存状態のまま不足内容を報告する。

- `As a / I want / so that` でユーザー価値を説明している
- 対象の `F-*`、`Q-*`、必要な `P-*` へ追跡できる
- 正常系、境界値、失敗系を含む観測可能な受入条件がある
- 対象外、依存関係、前提条件が明記されている
- QAがテスト方法と必要なfixtureをレビュー済みである
- 1回の実装スプリント内で完了できる大きさである
- 依存する `P-*` がPdMにより決定済みである
- PdMが優先順位を確定し、チームの見積もりがある

チェックボックスが付いているだけで満たしたと推定せず、Issue本文、リンク先、コメント、Project値で根拠を確認する。`Status = Ready` でも証拠が不足する項目は開始しない。

## 3. Planning案を作る

対象はReadyなleafの Feature/Enabler に限定する。Priority、依存順、見積もり、利用可能な担当者、スプリント容量から達成可能な候補を選ぶ。

- Engineerの `In progress` は1人1件を上限とし、現在のWIPを含める。
- QAの `QA` は1人1件を上限とし、今後の到着見込みも考慮して過積載を明示する。
- `Blocked` はWIPに含め、解除を新規着手より優先する。
- 親Storyの見積もりや進捗を実装容量へ加算せず、leafだけを集計する。
- 同じ親StoryのFeatureでも、依存順と独立して検証可能な薄い縦切りを優先する。
- 初回スプリントでProject上の優先順位とReady判定に反しない場合は `EN-001` を基準候補にする。

更新前に、Iteration、対象Issue、Ready根拠、Estimate、Engineer/QA、既知の依存・ブロッカーを表で提示する。ユーザーが対象、容量、担当、Iterationを既に指定し、Projectとも整合している場合は再確認を挟まず進める。これらに複数の妥当な選択肢があり、結果が変わる場合だけ質問する。

## 4. 開始状態へ更新する

書き込み直前に対象項目を再取得し、Ready状態、WIP、依存関係に競合する更新がないことを確認する。その後、Projectの実際のフィールドIDとOption/Iteration IDを解決して更新する。表示名からIDを推測したり、他ProjectのIDを再利用したりしない。

| フィールド | 開始時の値 |
| --- | --- |
| `Status` | `In progress` |
| `Work type` | 既存の `Feature` または `Enabler` を維持 |
| `Phase` | `Implementation` |
| `Implementation Sprint` | Planningで確定したIteration |
| `Estimate` | Ready判定で合意済みの1、2、3、5、8 |
| `Engineer` | 実装責任者 |
| `QA` | ペアとなるQAのGitHubユーザー名または合意済みエージェント名 |

`Story Sprint`、親Storyの進捗、`Priority` は上書きしない。親Storyを `In progress` にしてWIPやEstimateを二重計上しない。失敗した更新を成功扱いせず、途中まで更新された場合は項目ごとの実状態を報告して、勝手な一括ロールバックはしない。

## 5. 最初の作業を定義する

各項目について、EngineerとQAが並行して着手できる最小の開始作業をIssueコメントまたは開始報告にまとめる。

- Engineer: 受入条件から実装境界と最初のテストを定め、対象ブランチ/PR方針と必要な文書更新を明記する
- QA: 受入条件を正常系、境界値、失敗系へ対応付け、fixture、探索的テスト、OS/CLI互換性、必要な証跡を明記する
- 共通: 決定性、競合処理、ロールバック、macOS/Linux/Windows差異から該当リスクを選び、検証方法を定める

リポジトリに存在しない `build`、`test`、`lint`、`format` コマンドを想定しない。追加要求は対象Issueへ暗黙に含めず、新規Inbox候補として分離する。

## 完了報告

次を簡潔に報告する。

- 開始した `Implementation Sprint` と対象Issue
- Engineer/QAのペア、Estimate、最初の作業
- Ready判定の根拠と更新したProjectフィールド
- WIP、未解決の依存、ブロッカー、次の確認日
- 対象外とした候補、その未達条件
- 更新できなかった項目と必要な対応

Project上で更新結果を再取得でき、対象項目が意図した開始状態にあることを確認して初めて「開始済み」と表現する。
