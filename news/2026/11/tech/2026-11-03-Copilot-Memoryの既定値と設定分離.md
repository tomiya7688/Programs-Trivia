# Copilot Memory、Disabledだったのに記憶が許可されるんだぜ！？ 設定が二つに分かれたんだぜ！

- **掲載日**: 2026-11-03
- **出来事の日付**: 2026-10-28（Copilot Memoryの正式提供発表）、2026-10-29（設定画面についての観測）
- **確認日**: 2026-11-03
- **区分**: Tech News / GitHub Copilot・設定・プライバシー
- **対象**: GitHub Copilot Memory、個人の好み、リポジトリの記憶
- **注意**: **以前「Disabled」にしていたのに「Repository memory allowed」と表示されたという観測と、GitHub公式の仕様を分けて扱うんだぜ。設定の値が強制的に上書きされたと公式に確認されたわけではないぜ**

## おいおい、無効にしていたのに記憶が許可されてるんだぜ！？

「Copilot Memoryは使わないから、**Disabled**にしたんだぜ！」

ところが10月29日ごろ、設定画面を確認したら **「Repository memory allowed」** と表示されていたという話があるんだぜ。

**おいおい、勝手にオンになったんだぜ！？**

まず事実として、GitHubは **2026年10月28日にCopilot Memoryを正式提供（GA）** したんだぜ。Copilotが作業の中で得た知識を、次のセッションや対応する複数のCopilot製品でも活用する機能なんだぜ。

でも、今の仕様では**記憶が二種類あり、設定の管轄も別々**なんだぜ。

## 実は「個人の記憶」と「リポジトリの記憶」は違うんだぜ！

| 種類 | 何を記憶するんだぜ？ | どこで制御するんだぜ？ |
| --- | --- | --- |
| **User-level preferences** | 個人のコーディングの好み、回答形式、普段の作業方針なんだぜ | GitHubの**個人Copilot設定**なんだぜ |
| **Repository-level facts** | テストコマンド、設計方針、リポジトリの規約などなんだぜ | **リポジトリ設定・組織/企業のアクセス制御**なんだぜ |

GitHub公式ドキュメントでは、**個人のCopilot Memory設定でUser-level preferencesを無効にしても、Repository-level factsは無効にならない**と明記されているんだぜ。

しかも**リポジトリの記憶は、企業・組織・リポジトリの設定で制限しない限り、アクセスが許可されるのが既定値**なんだぜ。組織のRepository access設定の初期値は **All repositories** なんだぜ。

だから、**「個人の好みはDisabled」でも「Repository memory allowed」**という組み合わせは現在の仕様と矛盾しないんだぜ。

「記憶をオフにしたんだぜ！」→「そっちは個人の記憶のスイッチだったんだぜ！？」

……**これは見落としやすいんだぜ！**

## 以前のDisabledが上書きされたのは本当なんだぜ？

ここは慎重に区別するんだぜ。

今回の**「以前Disabledだったのに、Repository memory allowedになっていた」**という話は設定画面についての観測なんだぜ。

公式資料で確認できるのは、**現在は二種類の記憶を別のスイッチで管理する**という仕様なんだぜ。

**もともとの明示的なDisabled設定が移行で解除されたのか、設定の意味・表示対象が変わって別の許可状態が見えるようになったのか**は、公開資料だけでは確定できないんだぜ。

しかもGitHubは10月22日の**企業・組織向けグローバル既定ポリシー**の告知で、**個別機能について管理者が明示的に選んだEnabled/Disabledは維持する**と案内しているぜ。

ただし、その告知は企業・組織向けのグローバル既定ポリシーについてで、**個人のCopilot Memory設定が具体的にどう移行したかを説明する資料ではない**んだぜ。

「GitHubが全員の拒否設定を勝手に解除した」と断定するのも、「以前と全く同じ制御だ」と決めつけるのも早いんだぜ。

## 記憶させたくないなら、どこをオフにするんだぜ？

**個人の好み**を残さないなら、GitHubのプロフィールから **Copilot settings → Features → Copilot Memory** でUser-level preferencesを確認するんだぜ。

**特定のリポジトリの事実**を保存・利用させたくないなら、リポジトリ管理者が次を確認するんだぜ。

```text
Repository
  → Settings
    → Copilot
      → Memory
        → Allow Copilot to store and retrieve repository facts
```

ここをオフにすると、そのリポジトリについての新しい記憶の作成と利用が止まるんだぜ。

組織で管理している場合は **Organization Settings → Copilot → Policies → Copilot Memory → Repository access** も確認するんだぜ。**No repositories / Selected repositories / All repositories** の選択肢があるんだぜ。

さらにCLIでは `/memory show` で状態を確認し、`/memory off` で**そのCLIの両種類の記憶利用を止められる**んだぜ。ただしGitHub.comの設定そのものを変更するわけではないぜ。VS Codeにも `chat.copilot.memory.enabled` という別の設定があるんだぜ。

**「一か所だけDisabledなら全部オフ」ではない**というのが重要なんだぜ。

## オフにすれば保存済みの記憶も消えるんだぜ？

**いや、そこにも落とし穴があるんだぜ！**

GitHub公式の説明では、**リポジトリのMemoryをオフにすると、既存のリポジトリ情報は利用されなくなるが、自動削除はされない**んだぜ。

個人の好みも、Disabledへ切り替えるだけでは以前の記憶を削除しないんだぜ。

不要な記憶は、**個人のメモリ一覧**や、権限に応じて **Repository → Settings → Copilot → Memory** から確認して削除するんだぜ。

リポジトリの記憶は、同じリポジトリでCopilot Memoryを使えるコラボレーターと共有されるものなんだぜ。だから、パスワードや個人の秘密を保存する場所にしちゃいけないんだぜ。

## だからなんだぜ

Copilot Memoryの正式提供で、個人の好みとリポジトリの規約を分けて覚えられるようになったんだぜ。

便利だけど、**以前「Disabled」にしたという記憶だけで、すべてのメモリ利用が禁止されていると考えるのは危険**なんだぜ。

そして「Repository memory allowed」という表示を見ても、**個人設定の移行によって拒否が上書きされたと断定できるわけではない**んだぜ。

**「オフにしたんだぜ！」→「で、どっちの記憶をオフにしたんだぜ！？」**

……おいおい、**設定名だけじゃなく、適用範囲まで確認するんだぜ！**

### Sources

- [GitHub Changelog：GitHub intelligence puts your code, context, and history to work（2026-10-28）](https://github.blog/changelog/2026-10-28-github-intelligence-puts-your-code-context-and-history-to-work/) — Copilot Memoryの正式提供なんだぜ
- [GitHub Docs：About GitHub Copilot Memory](https://docs.github.com/en/copilot/concepts/prompting-and-context/copilot-memory) — 二種類の記憶、既定値と適用範囲なんだぜ
- [GitHub Docs：Managing Copilot Memory for your personal account](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory/manage-for-yourself) — 個人設定・CLI・VS Code・保存済み情報なんだぜ
- [GitHub Docs：Managing Copilot Memory for an enterprise, organization, or repository](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory/manage-as-administrator) — リポジトリと組織の設定、メモリの削除なんだぜ
- [GitHub Changelog：Global default for Copilot features generally available（2026-10-22）](https://github.blog/changelog/2026-10-22-global-default-for-copilot-features-generally-available/) — 管理者が明示的に設定した値を維持する告知なんだぜ
