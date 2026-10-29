# GitHub ActionsでAIエージェントを動かす仕組みが正式提供になったんだぜ！

- **掲載日**: 2026-10-29
- **出来事の日付**: 2026-10-28
- **区分**: Tech News / GitHub Actions / AIエージェント
- **対象**: GitHub Agentic Workflows
- **提供状況**: 一般提供（GA）

## ワークフローなのに、指示がMarkdownなんだぜ？

GitHubが2026年10月28日、**GitHub Agentic Workflowsを一般提供（GA）**したと発表したんだぜ！

普通のGitHub Actionsは「このコマンドを実行して、次にこれを実行して……」と処理を組み立てるんだぜ。でも今回の仕組みは、**自然言語のMarkdownで目的を記述し、それをGitHub ActionsのYAMLへコンパイルする**という発想なんだぜ。

たとえばIssueの振り分け、CI失敗の原因調査、ドキュメントの更新、テスト作成といった、判断を伴う仕事をエージェントに任せる仕組みなんだぜ。GitHub Nextから生まれたオープンソースのプロジェクトなんだぜ。

## Actionsだから、既存の運用に乗せられるんだぜ

生成されるのは標準的なActionsワークフローだから、既存のランナーグループやポリシーを利用するんだぜ。GitHub Copilotだけでなく、自分で用意したコーディングエージェントを使う選択肢もあるぜ。

一般提供に向けて、Actions Runner Controller（ARC）でのDocker-in-Docker対応、実行予算を見積もる `gh aw forecast`、モデル選択や隔離機能なども改善されたんだぜ。

ただ、AIが判断して操作するんだから、安全面を無視したら大変だぜ！

## 書き込みを無制限に許すわけじゃないんだぜ

GitHubの説明では、エージェントは**デフォルトで読み取り専用**、サンドボックス内で動作し、入力の整合性フィルター、Agent Workflow Firewall、結果を検証するsafe outputs、変更内容を検査する脅威検出ジョブなどを組み合わせているんだぜ。

とはいえ、**安全対策があることと、エージェントの判断や生成物が必ず正しいことは別**なんだぜ。特に権限を増やす運用やファイルを書き換える仕組みは、差分レビューやテストが必要だぜ。

## GAでも、何もかも無料とは限らないんだぜ

GitHubの発表では、github.comの**GitHub Free、Team、Enterprise**で一般提供なんだぜ。一方、ワークフローのエンジンとして標準の**GitHub Copilot CLIを使うにはCopilotライセンスが必要**なんだぜ。ランナーや他のモデル・エージェントの利用条件も、採用する構成によって変わるぜ。

**「Markdownを書けば何でも無料・無確認で自動実行できる」ではない**んだぜ。

## だからなんだぜ

CIが決められたコマンドを実行するだけだったところに、「状況を読んで、必要な作業を考える」という選択肢が増えたんだぜ。IssueやCIの運用を自動化したい開発者には、かなり面白い進展だぜ！

### Sources

- GitHub Changelog（2026-10-28、公式発表） — https://github.blog/changelog/2026-10-28-github-agentic-workflows-generally-available/
- GitHub Agentic Workflows（公式ドキュメント） — https://github.github.com/gh-aw/
- GitHub Agentic Workflows（公式ソース） — https://github.com/github/gh-aw
