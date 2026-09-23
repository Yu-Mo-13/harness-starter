---
name: start-implementation-sprint
description: Harness MVPの実装スプリントを計画・実行し、ReadyなFeatureとEnablerのコード、テスト、文書、PR、QA証跡を完成させる。Storyのリファインメントには使用しない。
---

# 実装スプリントを実行する

`docs/scrum-process.md` に従って Sprint Planningを行い、Definition of Readyを満たすFeature/Enablerを選定した後、実際にコード、テスト、文書を変更し、検証、PR作成、QA証跡の記録まで進める。対象と担当を決めたり `In progress` へ移したりしただけで終了しない。

明示的なスキル呼び出しは、選定したスプリント項目に必要なリポジトリ変更、ブランチ、コミット、PR、Issue、Projectの更新を依頼したものとして扱う。マージ、リリース、PdM受入、`Done` 化は、ユーザーが明示的に依頼するか必要な承認が確認できるまで行わない。

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

## 3. Planningと着手順

対象はReadyなleafの Feature/Enabler に限定する。Priority、依存順、見積もり、利用可能な担当者、スプリント容量から達成可能な候補を選ぶ。

- Engineerの `In progress` は1人1件を上限とし、現在のWIPを含める。
- QAの `QA` は1人1件を上限とし、今後の到着見込みも考慮して過積載を明示する。
- `Blocked` はWIPに含め、解除を新規着手より優先する。
- 親Storyの見積もりや進捗を実装容量へ加算せず、leafだけを集計する。
- 同じ親StoryのFeatureでも、依存順と独立して検証可能な薄い縦切りを優先する。
- 初回スプリントでProject上の優先順位とReady判定に反しない場合は `EN-001` を基準候補にする。

Iteration、対象Issue、Ready根拠、Estimate、Engineer/QA、依存順、既知のブロッカーを整理する。ユーザーが対象、容量、担当、Iterationを指定済みなら再確認を挟まない。複数の妥当な選択肢があり、結果が変わる場合だけ質問する。

選定した全項目へ `Implementation Sprint`、`Engineer`、`QA` を設定する。各Engineerにつき、今実際に作業する先頭の1件だけを `In progress` にし、後続項目は `Ready` のままスプリントキューに置く。先頭項目が `In review` 以降へ進むまで後続項目を `In progress` にしない。

## 4. 実装する

着手直前に対象項目を再取得し、Ready状態、WIP、依存関係に競合する更新がないことを確認する。Projectの実際のフィールドIDとOption/Iteration IDを解決して `Phase = Implementation` とし、作業対象だけを `Status = In progress` にする。表示名からIDを推測したり、他ProjectのIDを再利用したりしない。

Issueごとに焦点を絞ったブランチを作り、リポジトリの `AGENTS.md` と既存規約に従って次を実行する。

- Issue、親Story、Requirement IDs、受入条件を実装可能な変更単位へ対応付ける。
- 現在のコードとテストを調査し、既存設計を不必要に置き換えず最小の整合した実装を行う。
- 受入条件を実証する自動テストを実装と同時に追加する。正常系だけでなく、該当する境界値、失敗、決定性、競合、キャンセル、ロールバック、OS差異を扱う。
- CLI文言と生成テンプレート本文を英語に保つ。パス処理はplatform-neutralにし、秘密情報やリポジトリ内容をログへ露出しない。
- 変更により要件、Feature ID、Decision、利用方法が変わる場合は関連文書を同じ変更で更新する。
- リポジトリに存在する `build`、`test`、`lint`、`format` のみ実行する。コマンドが存在しない場合は捏造せず、未整備として記録する。
- 失敗したテストを無効化したり、要件を弱めたりして通過扱いにしない。スコープ外の要求は新規Inbox候補として分離する。

作業中にブロックされた場合は `Status = Blocked` とし、理由、解除条件、次の確認日をIssueへ記録する。失敗した更新や部分実装を完了扱いせず、安全に保持できない変更を勝手に破棄しない。

## 5. 自己レビューとPR

実装後は差分全体を受入条件とDefinition of Doneに照らしてレビューし、不要な変更、追跡漏れ、テスト不足、プラットフォーム依存、秘密情報、後方互換性リスクを修正する。実行したコマンドと結果を記録する。

検証可能な状態になったらConventional Commit形式でコミットし、`.github/PULL_REQUEST_TEMPLATE.md` を満たすPRを作成する。PRには対象Issue、Feature/Quality/Decision IDs、受入条件との対応、コマンド結果、リスク、ロールバック、CLI出力またはスクリーンショットを記録する。PR作成を確認した後に対象を `In review` へ移す。

## 6. QA作業を行う

Engineerの実装観点とは分けて、QAとして実際の差分と成果物を検証する。

- 各受入条件を自動テストまたは探索的テストの証拠へ対応付ける。
- 必要なfixtureを使い、正常系、境界値、失敗系を実行する。
- 該当するmacOS、Linux、Windows、Node.js、Codex/Claude Code CLIの結果を確認する。実行していない環境を成功扱いしない。
- CLI変更は終了コード、標準出力・標準エラー、非TTY、`NO_COLOR`、キーボード操作、機密値の非表示を必要に応じて確認する。
- 重大な問題は再現手順と期待結果をPRへ記録して修正し、修正後に関連テストを再実行する。

自動チェックと必要なレビューが成功し、マージ候補に対する探索的テストを開始できる段階で `Status = QA` にする。QA結果と証跡をPRへ追記する。すべてのDoDを確認できても、PdMの受入前に `Done` へ移さない。

一つの項目が `In review` 以降へ進んだらWIPを再取得し、容量があれば同じEngineerの次のReady項目へ進む。選定した項目を順に実装し、単に一覧を作った時点でスプリント作業を終えない。

## 完了報告

次を簡潔に報告する。

- 実行した `Implementation Sprint`、対象Issue、Engineer/QA、Estimate
- 実装したコード、テスト、文書と受入条件への対応
- 実行したチェック、QA結果、未実施環境
- コミット、PR、Projectの現在状態
- `Blocked` または未完了の項目、解除条件、次の確認日
- PdM受入、マージなど残る外部アクション

リポジトリ、PR、Issue、Projectを再取得し、報告内容と実状態が一致することを確認する。対象を `In progress` に移しただけでは完了としない。
