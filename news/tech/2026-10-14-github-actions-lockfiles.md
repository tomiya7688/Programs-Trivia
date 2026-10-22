# GitHub Actionsにlockfileが来たんだぜ！

- **掲載日**: 2026-10-22
- **出来事の日付**: 2026-10-14
- **区分**: Tech News / GitHub Actions / セキュリティ
- **対象**: GitHub Actions workflow dependency lockfiles（パブリックプレビュー）

## CIで動くコード、実は毎回同じとは限らないんだぜ

GitHubが2026年10月14日、**GitHub Actionsのワークフロー依存関係をlockfileで固定する機能**をパブリックプレビューとして発表したんだぜ。

GitHub Actionsでは、こんな指定をよく見かけるんだぜ。

~~~yaml
steps:
  - uses: actions/checkout@v4
~~~

読みやすくて便利なんだぜ。

でもな。

**`@v4`のようなタグが、いつまでも同じコミットを指すとは限らない**んだぜ。

リポジトリ側のワークフローを一行も変更していなくても、タグの参照先が変われば別のコードが動く可能性があるんだぜ。
これは再現性の問題だけじゃなく、依存先が乗っ取られた場合のサプライチェーン攻撃にも関わるんだぜ。

## `actions.lock`で実行するコミットを固定するんだぜ

新機能では、リポジトリの **`.github/workflows/actions.lock`** に依存アクションの情報を記録するんだぜ。

~~~text
ワークフローでタグを指定するんだぜ
    ↓
gh actions-lockで依存関係を調べるんだぜ
    ↓
actions.lockへコミットを記録するんだぜ
    ↓
実行時には記録されたコミットを使うんだぜ
~~~

こうすると、ワークフローの`uses:`には読みやすいタグを残しつつ、実際に動かすコードを固定できるんだぜ。

しかも、**間接的に使われるアクションの依存関係**も対象になるんだぜ。
ただし再利用可能ワークフローの内部依存関係は、そのワークフローを定義した側のリポジトリのlockfileが受け持つんだぜ。

## Dependabotとの連携もあるんだぜ

lockfileを導入済みのワークフローについて、Dependabotがアクションのバージョン更新PRを作ると、対応するlockfileの内容も更新できるんだぜ。

だから、

> 固定したら更新が面倒になって、そのまま放置なんだぜ。

とならないように工夫されているんだぜ。

lockfileが古くなっていないかを検証する機能もあるんだぜ。
CIの変更をレビューする際に、**依存するコードが本当に変わったのか**を見やすくなるのがうれしいんだぜ。

## すべてのGitHub Actionsで自動的に有効になるわけじゃないんだぜ

今回発表されたのは、あくまで**パブリックプレビュー**なんだぜ。

GitHubの発表では、Enterprise Cloud、Enterprise Cloud with data residency、Team、Freeの各プランが対象として案内されているんだぜ。
ただしプレビュー機能なので、仕様や提供条件が変わる可能性はあるんだぜ。

利用するときは、GitHub CLIの **`gh actions-lock`拡張**を導入してlockfileを生成・確認・コミットするんだぜ。
ファイルを手作業で書き換えるものではないんだぜ。

そして、**lockfileに書かれたコードが安全だと保証されるわけではない**んだぜ。
固定する前に依存先を確認し、更新時の差分をレビューする必要があるぜ。

## だからなんだぜ

今までもGitHub ActionsではコミットSHAを直接指定する方法が推奨されていたんだぜ。

今回の仕組みは、**ワークフローの可読性と、実行コードを固定する安全性の両立**を狙っているんだぜ。

CIをたくさん動かすプロジェクトほど、一度調べてみる価値があるニュースなんだぜ！

### Sources

- GitHub Changelog（2026-10-14） — https://github.blog/changelog/2026-10-14-github-actions-workflow-lockfiles-in-public-preview/
- GitHub Docs, *Workflow dependency lockfiles* — https://docs.github.com/en/enterprise-cloud@latest/actions/concepts/security/workflow-dependency-lockfiles
- GitHub, *gh-actions-lock* — https://github.com/github/gh-actions-lock
