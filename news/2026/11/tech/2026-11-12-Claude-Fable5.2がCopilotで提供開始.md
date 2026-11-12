# Claude Fable 5.2がCopilotに来たんだぜ！ でもデータ保持に注意なんだぜ！

- **掲載日**: 2026-11-12
- **出来事の日付**: 2026-11-09（GitHubがCopilotでの一般提供を告知した日なんだぜ）
- **区分**: Tech News / AIコーディング・モデル提供
- **対象**: Anthropic Claude Fable 5.2 / GitHub Copilot
- **提供状態**: 一般提供（GA）だが、段階的ロールアウトとプラン・企業ポリシーの条件があるんだぜ
- **検証範囲**: GitHub公式発表とAnthropic公式ヘルプで確認したぜ。Triva独自の品質・速度の比較ではないんだぜ

## また強いモデルが来たんだぜ！？

2026年11月9日、GitHubが**Claude Fable 5.2をGitHub Copilotで一般提供**すると発表したんだぜ！

大きなコードベースを扱う複数段階の開発作業を想定しているんだぜ。GitHubの初期テストでは、Fable 5.1より**少ない手順・ツール呼び出しで多くのタスクを解いた**と説明しているんだぜ。

ただし、これは開発元の初期評価なんだぜ。あらゆるプロジェクトで処理速度や品質が必ず向上するという独立検証ではないんだぜ。

## エディタだけじゃないんだぜ

GitHubの発表では、VS Code、Visual Studio、Copilot CLI、Copilot coding agent、Copilotアプリ、github.com、GitHub Mobile、JetBrains IDE、Xcode、Eclipseを対象としているんだぜ。

対応契約は**Copilot Pro+、Max、Business、Enterprise**なんだぜ。対象者へ段階的にロールアウトされるので、モデル選択欄に表示される時期は人によって違う可能性があるぜ。

Business・Enterpriseでは**管理者のFable 5.2モデルポリシーが既定でオフ**なんだぜ。管理者が明示的に許可しない限り使えないぜ。

請求は**使用量ベース課金でプロバイダーの定価を採用**すると案内されているんだぜ。「Copilotに来たから追加負担なしで無制限」と考えるのは違うんだぜ。現行の料金表と契約条件を見て判断する必要があるぜ。

## 「学習に使われない」と「保持されない」は別物なんだぜ

特に大事なのがデータの扱いなんだぜ。

GitHubは**Fable 5.2の利用時には、安全性監視のためAnthropicが入力と出力を原則保持する**と説明しているんだぜ。Anthropicの公式ヘルプでは、この対象モデル群について通常**30日間**保持し、例外として安全性上のフラグや法的要請により長く保存される場合があると説明しているぜ。

GitHubによると、保持データは**Anthropicのモデル学習には使われない**んだぜ。でも、機密コードを送っても保存されないという意味にはならないんだぜ。

他のClaudeモデルの多くはCopilot側でゼロデータ保持（ZDR）の条件を維持する一方、**Fable 5 / 5.1 / 5.2は例外**なんだぜ。

一部の条件を満たす企業には、**2026年末までの期限付きZDRアクセス**が用意されているぜ。その後はAnthropicのEnterprise Frontier Safeguards（EFS）が必要だとGitHubは説明しているんだぜ。

対象企業でも認定と管理者によるモデル有効化は別。機密情報を扱う組織では、採用前に権限とデータ保持規則を確認するんだぜ。

## だからなんだぜ

モデルの性能だけでは、利用サービスの実際の価値は決まらないんだぜ。

**モデル品質・対象プラン・従量課金・情報保持・管理者ポリシー**まで含めて評価する必要があるニュースなんだぜ！

### Sources

- GitHub公式Changelog（2026-11-09）、Claude Fable 5.2 in GitHub Copilot — https://github.blog/changelog/2026-11-09-claude-fable-5-2-in-github-copilot/
- Anthropic公式ヘルプ、Data retention practices for Covered Models（2026-11-12確認） — https://support.claude.com/en/articles/15425996-data-retention-practices-for-covered-models
