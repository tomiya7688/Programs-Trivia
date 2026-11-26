# GitHub Issuesに7つの状態が来たんだぜ！ MCP対応のRepository Boardsもプレビューだぜ！

- **出来事の日付**: 2026-11-23（GitHubのPublic Preview発表）
- **執筆日**: 2026-11-26
- **区分**: Tech News / GitHub Issues・AIエージェント
- **公開状態**: Public Previewなんだぜ

## OpenかClosedだけでは足りないんだぜ！

GitHub Issuesの状態といえば、これまでは基本的にOpenかClosedだったんだぜ。

でも実際の開発では、まだ着手していないのか、作業中なのか、レビュー中なのか、完了したのかを区別したいんだぜ。

そこでGitHubは2026年11月23日、Issueに細かな状態を付ける機能と、リポジトリ内で進捗を見られるRepository Boardsを**Public Preview**として発表したんだぜ！

## Issueに7つの状態を付けられるんだぜ！

今回用意された状態は次の7つなんだぜ。

| 状態 | ざっくりした意味 |
| --- | --- |
| Backlog | まだ順番待ち |
| Todo | 次に取りかかる |
| In progress | 作業中 |
| In review | レビュー中 |
| Done | 完了 |
| Not planned | 対応しない |
| Duplicate | 重複 |

Issueを作るときや、Issueのヘッダーから状態を選べるんだぜ。リポジトリのIssue一覧やダッシュボード、Repository Boardsにも状態が表示されるんだぜ。

## PRや担当者に合わせて状態も動くんだぜ！

さらに面白いのが自動更新なんだぜ。

- Draft PRをIssueに紐付けるとIn progress
- Draft PRをReady for reviewにするとIn review
- Open PRをIssueに紐付けてもIn review
- 人間を担当者にするとTodo
- **AIエージェントを担当者にするとIn progress**
- GitHub Projects側に同じ状態名があれば、IssueとProjectの状態が同期する

つまり、IssueをAIエージェントへ割り当てたときにも、作業中であることが一覧に反映されるんだぜ。

しかもGraphQL・REST API・MCPサーバーから状態を読み書きできるんだぜ。エージェントはIssueの状態を取得したり、変更したり、新しいIssueの初期状態を指定できるんだぜ。状態変更はCopilot automationsのトリガーにも使えるんだぜ。

ただし、**MCPで状態を操作できることと、エージェントにリポジトリの全操作を許可することは別**なんだぜ。実際に何ができるかは、接続先と権限設定を確認するんだぜ。

## Repository BoardsでIssueを並べて見られるんだぜ！

リポジトリの保存済みビューに、ボード形式の表示が加わったんだぜ。

- 状態ごとにIssueを列へ分ける
- Issueを別の列へドラッグして状態を変更
- 列、スイムレーン、表示項目、並び順を調整

これならGitHub Projectsを開かなくても、リポジトリ内で簡単な進捗確認ができるんだぜ。より高度な計画管理が必要なら、従来どおりGitHub Projectsを使うんだぜ。

それからIssue fieldsもIssue typesから独立して扱えるようになったんだぜ。Priority以外のフィールドも、Issue typeへ紐付けなくても追加できるようになるぜ。

## まだPublic Previewなんだぜ！

今回の機能は**一般提供ではなくPublic Preview**なんだぜ。利用可能な範囲や仕様が今後変わる可能性があるから、既存の運用ルールをすぐ全部置き換えるのではなく、まずは小さなリポジトリで試すのがよさそうだぜ。

## だからなんだぜ

GitHub Issuesが、単なる「開いているか閉じているか」の一覧から、**人間とAIエージェントが共有できる作業状態の管理場所**へ進み始めたんだぜ。

特に、担当者へのエージェント割り当てでIn progressへ自動更新されること、MCP・REST・GraphQLから状態を扱えることは、AIを含む開発フローを組むときに注目したい変化なんだぜ！

### Sources

- GitHub Changelog（2026-11-23）, *Issue statuses and repository boards are in public preview* — https://github.blog/changelog/2026-11-23-issue-statuses-and-repository-boards-are-in-public-preview/
