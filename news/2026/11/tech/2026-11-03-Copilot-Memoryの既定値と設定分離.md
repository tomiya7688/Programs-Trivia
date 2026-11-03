# Copilot Memory、2択が4択に！？ Disabledが記憶許可の表示に変わったんだぜ！

- **掲載日**: 2026-11-03
- **出来事の日付**: 2026-10-28（Copilot Memoryの正式提供・公式ドキュメント更新）、2026-10-29（2択→4択と以前のDisabledの表示変化についての観測）
- **確認日**: 2026-11-03
- **区分**: Tech News / GitHub Copilot・設定・プライバシー
- **対象**: GitHub Copilot Memory、個人の好み、リポジトリの記憶
- **注意**: **従来は2択だったCopilot Memory設定が4択になり、以前のDisabledが「Repository memory allowed」と表示されていたという利用者の観測なんだぜ。GitHub公式ドキュメントの旧2択と10月28日の説明変更は確認できたが、どの保存値をどう移行したかは未確認なんだぜ**

## おいおい、無効にしていたのに記憶が許可されてるんだぜ！？

「Copilot Memoryは使わないから、**Disabled**にしたんだぜ！」

ところが**2026年10月29日ごろ、設定が2択から4択へ変わったあと**に同じ項目を開くと、**以前はDisabledを選んでいたのに「Repository memory allowed」へ変わっていた**という観測があったんだぜ。

**おいおい、以前のオプトアウトが部分的に許可へ変わったように見えるんだぜ！？**

まず事実として、GitHubは **2026年10月28日にCopilot Memoryを正式提供（GA）** したんだぜ。Copilotが作業の中で得た知識を、次のセッションや対応する複数のCopilot製品でも活用する機能なんだぜ。

ここで重要なのは、**以前の単一の2択と、現在の個人・リポジトリ別の許可状態は意味が異なる**ことなんだぜ。GitHub公式ドキュメントの旧版と新版も比較できるんだぜ。

## 実は「個人の記憶」と「リポジトリの記憶」は違うんだぜ！

| 種類 | 何を記憶するんだぜ？ | どこで制御するんだぜ？ |
| --- | --- | --- |
| **User-level preferences** | 個人のコーディングの好み、回答形式、普段の作業方針なんだぜ | GitHubの**個人Copilot設定**なんだぜ |
| **Repository-level facts** | テストコマンド、設計方針、リポジトリの規約などなんだぜ | **リポジトリ設定・組織/企業のアクセス制御**なんだぜ |

GitHub公式ドキュメントでは、**個人のCopilot Memory設定でUser-level preferencesを無効にしても、Repository-level factsは無効にならない**と明記されているんだぜ。

しかも**リポジトリの記憶は、企業・組織・リポジトリの設定で制限しない限り、アクセスが許可されるのが既定値**なんだぜ。組織のRepository access設定の初期値は **All repositories** なんだぜ。

だから、**新しい仕様では「個人の好みはDisabled」でも「Repository memory allowed」という状態自体は成立する**んだぜ。

ただし、**以前の単一のDisabledは両方の記憶を対象にしていたと旧公式文書で説明されていた**んだぜ。今回の話を単に「以前から違う設定だっただけ」と片付けちゃいけないんだぜ。

## 以前のDisabledは引き継がれたんだぜ？ 公式文書を比較するぜ！

**2026年10月28日より前のGitHub公式文書**には、Copilot Memoryのドロップダウンは**Enabled / Disabledの2択**と書かれていて、機能を有効にすると**リポジトリの情報と個人の好みの両方が保存される**と説明されていたんだぜ。

ところが、**2026年10月28日のGitHub公式ドキュメント更新**では、個人のCopilot Memory設定は**User-level preferencesを制御し、Repository-level factsは制御しない**という説明に変更されたんだぜ。

| 証拠 | 確認できること |
| --- | --- |
| [変更前の公式文書](https://github.com/github/docs/blob/e0679b208251b643fdf7cbcc1c6189caa16016c4/content/copilot/how-tos/use-copilot-agents/copilot-memory/manage-for-yourself.md) | **2択**で、両方の記憶を対象とする説明だったんだぜ |
| [2026年10月28日の変更](https://github.com/github/docs/commit/8e7396e5e2e02bac9caba10c8d944f938bcd9b52) | 個人設定が扱うのは**個人の好みだけ**になり、リポジトリ側と区別されたんだぜ |
| **10月29日ごろの利用者の観測** | 設定が**4択になり、従来Disabledだった箇所にRepository memory allowedと表示された**んだぜ |

**文書の意味が変わったことは確認できるけど、保存済みのDisabledが内部的にどの値へ移行されたのかは、公式の移行仕様を確認できない**んだぜ。

しかも現行のGitHub公式ドキュメントには、**既存のオプトアウト設定は引き続き有効**だという説明もあるんだぜ。これが旧2択のDisabledを4択へどう写すという意味なのかは明記されていないから、**表示変更だけなのか、実際の許可範囲まで変わったのかは公開資料だけでは確定できない**んだぜ。

つまり、以前のオプトアウトを意図した人から見れば**許可範囲が広がったように見える**、という問題提起は重要なんだぜ。「値を強制的に変更した」と断定するには、設定の変更履歴や公式説明など追加の証拠が必要なんだぜ。

しかもGitHubは10月22日の**企業・組織向けグローバル既定ポリシー**の告知で、**個別機能について管理者が明示的に選んだEnabled/Disabledは維持する**と案内しているぜ。

ただし、その告知は企業・組織向けのグローバル既定ポリシーについてで、**個人のCopilot Memory設定が具体的にどう移行したかを説明する資料ではない**んだぜ。

**以前の2択から4択への移行を、古いオプトアウトの意味まで含めて検証する必要がある**んだぜ。

2026年11月3日時点で公開されているGitHub Issues・Community・Redditを調べた範囲では、**この2択→4択とDisabled→Repository memory allowedを同時に報告する独立した事例は確認できなかった**ぜ。過去の[VS Codeでメモリ無効化後もツールが動いた報告](https://github.com/microsoft/vscode/issues/318131)や[Copilot Memoryを無効化できないという相談](https://github.com/orgs/community/discussions/197045)はあるけど、**同じ原因の証拠ではない**んだぜ。

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

便利だけど、**昔の2択でDisabledを選んでいた人は、4択化後の実際の許可状態を確認したほうがいい**んだぜ。

とくに**DisabledからRepository memory allowedへ変わったという観測は、旧設定の意味が新しい設定へどう引き継がれたのかという問題**なんだぜ。表示上の変化は具体的だが、実際に記憶が生成・利用されたか、内部データが書き換わったかは別に検証する必要があるぜ。

**「オフにしたんだぜ！」→「で、どっちの記憶をオフにしたんだぜ！？」**

……おいおい、**設定名だけじゃなく、適用範囲まで確認するんだぜ！**

### Sources

- [GitHub Changelog：GitHub intelligence puts your code, context, and history to work（2026-10-28）](https://github.blog/changelog/2026-10-28-github-intelligence-puts-your-code-context-and-history-to-work/) — Copilot Memoryの正式提供なんだぜ
- [GitHub Docs：About GitHub Copilot Memory](https://docs.github.com/en/copilot/concepts/prompting-and-context/copilot-memory) — 二種類の記憶、既定値と適用範囲なんだぜ
- [GitHub Docs：Managing Copilot Memory for your personal account](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory/manage-for-yourself) — 個人設定・CLI・VS Code・保存済み情報なんだぜ
- [GitHub Docs：Managing Copilot Memory for an enterprise, organization, or repository](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/copilot-memory/manage-as-administrator) — リポジトリと組織の設定、メモリの削除なんだぜ
- [GitHub Changelog：Global default for Copilot features generally available（2026-10-22）](https://github.blog/changelog/2026-10-22-global-default-for-copilot-features-generally-available/) — 管理者が明示的に設定した値を維持する告知なんだぜ
- [変更前のGitHub Docs（2026-06-02時点のコミット）](https://github.com/github/docs/blob/e0679b208251b643fdf7cbcc1c6189caa16016c4/content/copilot/how-tos/use-copilot-agents/copilot-memory/manage-for-yourself.md) — 有効・無効の2択と、両種類の記憶を保存するという以前の説明なんだぜ
- [GitHub Docsの2026-10-28の更新コミット](https://github.com/github/docs/commit/8e7396e5e2e02bac9caba10c8d944f938bcd9b52) — 個人設定とリポジトリの記憶の範囲が変更された証拠なんだぜ
- [VS Code Issue #318131](https://github.com/microsoft/vscode/issues/318131) — メモリ無効化に関する別の不具合報告。今回と同一原因とは未確認なんだぜ
- [GitHub Community #197045](https://github.com/orgs/community/discussions/197045) — メモリをオフにできないという別件の相談で、今回の移行とは区別するぜ
