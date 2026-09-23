# Harness MVP スクラム運用

## 1. 目的

本書は、[要件定義](./requirements.md)、[機能一覧](./feature-list.md)、[残課題](./open-issues.md)を、実装可能なプロダクトバックログと検証済みのインクリメントへ変換する運用を定義する。

## 2. 役割

| 役割 | 責任 |
| --- | --- |
| プロダクトマネージャー兼オーケストレーター | Product Goal、優先順位、スコープ、`P-*`の意思決定、レビューでの受入判断 |
| スクラムマスター | セレモニーの進行、障害の可視化、WIP制御、メトリクスとレトロスペクティブの改善追跡 |
| エンジニア | ストーリー作成・リファインメント、設計、実装、自動テスト、レビュー対応 |
| QAエンジニア | 受入条件とテスト戦略のレビュー、探索的テスト、クロスプラットフォーム検証、品質リスクの提示 |

エンジニアとQAエンジニアは同数とし、ストーリーごとに1対1のペアを明示する。品質の最終責任はチーム全体が持つ。

## 3. スプリントモデル

同じ長さのスプリントを使用し、各バックログ項目を原則として次の2段階で進める。

1. ストーリースプリント: エンジニアがストーリー、受入条件、依存関係、見積もりを作成し、QAがテスト可能性をレビューする。
2. 実装スプリント: エンジニアとQAのペアが実装、自動テスト、探索的テスト、レビュー用証跡を完成させる。

パイプラインが安定した後は、スプリント`N`で次候補をリファインしながら、前スプリントまでにReadyになった項目を実装レーンで扱える。ただし、Readyでない項目を実装へ投入しない。

ProjectではStory/Decisionに`Story Sprint`、実装するFeature/Enablerに`Implementation Sprint`を設定する。両方を別のIterationフィールドとして保持し、Story作成工程の履歴を実装開始時に上書きしない。

各スプリントの開始時にPlanning、毎営業日に15分以内のDaily Scrum、終了時にReviewとRetrospectiveを行う。ReviewにはPdM、スクラムマスター、担当エンジニア、QAが参加し、PdMが受入を判断する。Retrospectiveにはスクラムチーム全員が参加する。改善アクションは1〜2件に絞り、担当者と期限を付けて次スプリントのバックログへ登録する。

## 4. ワークフロー

GitHub Projectsの`Status`は次を使用する。

| Status | 完了条件 |
| --- | --- |
| Inbox | 新規要求として記録され、重複確認待ち |
| Refinement | ストーリーと受入条件を作成中 |
| Ready | Definition of Readyを満たし、PdMが優先順位を確定済み |
| In progress | 実装担当とQA担当が決まり、作業中 |
| In review | PRが作成され、自動チェックとレビューを実施中 |
| QA | マージ候補に対する受入・探索的テスト中 |
| Done | Definition of Doneを満たし、PdMが受入済み |
| Blocked | 理由、解除条件、次の確認日がIssueに記録済み |

WIP上限は`In progress`をエンジニア1人につき1件、`QA`をQAエンジニア1人につき1件とする。Blocked項目はWIPに含め、ブロッカー解消を新規着手より優先する。QA完了後はSprint Reviewを待たず、PdMが継続的に受入判断して`Done`へ移す。Sprint ReviewではDoneになったインクリメントを検査し、次の適応を決める。

## 5. Definition of Ready

実装スプリントへ投入するには、次をすべて満たす。

- `As a / I want / so that`でユーザー価値が説明されている
- 対象の`F-*`、`Q-*`、必要なら`P-*`へ追跡できる
- 正常系、境界値、失敗系を含む観測可能な受入条件がある
- 対象外、依存関係、前提条件が明記されている
- QAがテスト方法と必要なfixtureをレビューしている
- 1回の実装スプリント内で完了できる大きさである
- 依存する`P-*`がPdMにより決定されている
- PdMが優先順位を確定し、チームが見積もっている

## 6. Definition of Done

- 受入条件を満たし、関連する自動テストが追加されている
- 決定性、競合処理、ロールバック、対象OS差異など該当する品質リスクを検証している
- `build`、`test`、`lint`、`format`など、リポジトリに存在する必須チェックが成功している
- QAの探索的テスト結果と、必要なCLI出力またはスクリーンショットがPRに記録されている
- ユーザー向け文言と生成テンプレート本文は英語である
- 関連文書、Feature ID、意思決定記録が更新されている
- 別のエンジニアによるレビューを受け、未解決の重大な指摘がない
- PdMが受け入れ、GitHub Projectsの必須フィールドが更新されている

## 7. GitHub Projects設定

[Harness MVP Project](https://github.com/users/Yu-Mo-13/projects/7)をリポジトリへ連携し、次のフィールドを設ける。

| フィールド | 種類 | 値 |
| --- | --- | --- |
| Status | Single select | ワークフローで定義した8状態 |
| Priority | Single select | P0、P1、P2 |
| Work type | Single select | Story、Decision、Bug、Spike、Enabler、Feature、Improvement |
| Phase | Single select | Story、Implementation、Validation |
| Story Sprint | Iteration | Story/Decisionを作成・Ready化する2週間。Sprint 1は2026-09-28開始 |
| Implementation Sprint | Iteration | Feature/Enablerを実装・QAする2週間。Sprint 1は2026-09-28開始 |
| Estimate | Number | 1、2、3、5、8 |
| Engineer | Assignees | 実装責任者 |
| QA | Text | QA担当のGitHubユーザー名 |
| Requirement IDs | Text | 対象の`F-*`、`Q-*`、`P-*` |
| Target | Single select | CLI、Detection、Selection、Template、Adapter、Filesystem、Validation、Release |

次のViewを作成する。

- `Product backlog`: Done以外をPriority順に表示するTable
- `Story creation`: 現在のStory SprintをStatus別に表示するBoard
- `Implementation sprint`: 現在のImplementation SprintをStatus別に表示するBoard
- `Refinement`: StatusがInboxまたはRefinementの項目をPriority順に表示するTable
- `QA`: StatusがIn reviewまたはQAの項目をQA担当別に表示するBoard
- `Decisions`: Work typeがDecisionの項目をPriorityとStatusで表示するTable
- `Roadmap`: Sprintを横軸、TargetをグループにしたRoadmap

Issue追加時は`Inbox`とする。実装対象のFeature/Enablerだけを、PR作成時に`In review`へ移す。PRのマージだけでは`Done`にせず、QA確認とPdM受入後に完了させる。

初期バックログには、既存のFeature Issue 53件、Enabler/Story Issue 13件、Decision Issue 17件の合計83件を登録している。StoryとDecisionは上位の価値・判断を表し、Featureは`F-001`〜`F-053`の実装単位を表す。

### 7.1 StoryとFeatureの集計ルール

- `US-001`〜`US-011`のSub-issueとして、対応する`F-001`〜`F-053`を一意に設定する
- Storyは価値、受入条件、Story Sprintを管理し、Featureは実装、Implementation Sprint、WIPを管理する
- 実装Sprintの見積もりと進捗集計はleafであるFeature/Enablerを用い、親Storyと二重計上しない
- Storyの初期見積もりはリファインメント時の分割判断にだけ使用し、実装容量には合算しない
- 親StoryをDoneにするのは、必須Sub-issueがすべてDoneで、Storyレベルの受入条件を満たした後とする
- 横断品質を扱う`US-012`は、各Featureの受入条件とCIへ適用し、単独で実装完了を代替しない
- `Assignees`には説明責任を持つGitHubユーザーを設定し、`QA`には担当するQA役またはエージェント名を記録する。QA ViewはStatus別に運用する

## 8. バックログ管理ルール

- Issue作成には[ユーザーストーリーテンプレート](../.github/ISSUE_TEMPLATE/user-story.yml)を使用する
- Product backlogの初期候補は[プロダクトバックログ](./product-backlog.md)を正とする
- `P-*`はDecision Issueとして登録し、決定内容と日付を[残課題](./open-issues.md)へ反映する
- 実装中の追加要求は現在のストーリーへ暗黙に加えず、新規Inbox項目としてPdMが優先順位を判断する
- Story pointは時間換算せず、チーム内の相対見積もりにだけ使用する
- バーンアップ、スループット、サイクルタイム、持ち越し率を観測する。ベロシティを個人評価に使用しない

## 9. 最初に構築する仕組み

1. GitHub Projectsのフィールド、View、自動化
2. TypeScript CLIの基盤、lockfile、`build`、`test`、`lint`、`format`とOS別CI
3. Branch protectionまたはRulesetによるPR必須化と必須チェック
4. `CODEOWNERS`と[PRテンプレート](../.github/PULL_REQUEST_TEMPLATE.md)によるエンジニア・QAレビューの明確化
5. ADRまたはDecision Issueによる`P-*`の決定履歴
6. テストfixture方針と、決定性・競合・ロールバック・OS差異の回帰テスト
7. リリース時のCodex/Claude Code実CLI互換性マトリクス
8. Dependabot等の依存関係更新と、秘密情報・ライセンス・脆弱性の検査
