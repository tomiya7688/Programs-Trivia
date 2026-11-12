# AIに古いWebを書かせないんだぜ！ Modern Web Guidance 1.0が正式版だぜ！

- **掲載日**: 2026-11-12
- **正式版の発表日**: 2026-11-10（Google Chrome開発チームによる1.0発表なんだぜ）
- **注目状況の確認日**: 2026-11-12（GitHub TrendingのToday一覧に掲載されていたぜ）
- **区分**: GitHub Trending / 注目リポジトリ・AI開発支援
- **対象**: [GoogleChrome/modern-web-guidance-src](https://github.com/GoogleChrome/modern-web-guidance-src) / Modern Web Guidance 1.0
- **配布用の別リポジトリ**: https://github.com/GoogleChrome/modern-web-guidance
- **ライセンス**: Apache-2.0（ソースリポジトリなんだぜ）
- **提供状態**: 1.0正式版。ただしブラウザ互換性とコード検証は別途必要なんだぜ
- **検証範囲**: Google公式ブログとドキュメント、公式ソース、GitHub Trendingを確認したぜ。Trivaの独立ベンチマークではないんだぜ

## AIが古いJavaScriptを書いてくるんだぜ！？

2026年11月10日、Google Chromeチームが**Modern Web Guidance 1.0**を正式公開したんだぜ。5月の早期プレビューから進んだ新バージョンなんだぜ！

AIコーディングエージェントは便利だけど、HTML・CSSの新しい仕組みで解決できるものを、古いJavaScriptで頑張って実装してしまうことがあるんだぜ。

例えば、Popover API、Dialog、CSS Anchor Positioning、View Transitions、コンテナクエリ。**ブラウザ自身に標準機能があるなら、そちらの方が短く安全な設計になる場合もある**んだぜ。

11月12日のGitHub TrendingのToday一覧で、開発元の**GoogleChrome/modern-web-guidance-src**の掲載も確認したぜ。順位とスター数は変動するので、固定順位を断定しないんだぜ。

なお、このリポジトリはガイドの**ソースと評価・開発環境**なんだぜ。エージェントが導入する配布用のリポジトリとは別なんだぜ。

## 強いモデルを作るんじゃなく、正しい専門知識を渡すんだぜ

Modern Web GuidanceはLLMモデルではなく、**ブラウザ仕様・互換性・アクセシビリティなどのガイドをAIエージェントへ渡すスキルとCLI**なんだぜ。

公式に案内された基本の使い方はこんな感じだぜ。

~~~bash
npx modern-web-guidance@latest install
npx modern-web-guidance@latest search "animate a dialog modal backdrop"
npx modern-web-guidance@latest retrieve "animate-to-from-top-layer"
npx modern-web-guidance@latest legacy-scan .
~~~

検索とガイド取得だけなら、インストール前にもCLIで調べられるんだぜ。導入する際はプロジェクトのルールに合わせて使うんだぜ。

扱うテーマは、ダイアログやCSSだけじゃないぜ。**キーボード操作・スクリーンリーダー、Webパフォーマンス、Passkeys、Content Security Policy、フォールバック**にも及ぶんだぜ。

## 古い実装を探すワークフローも増えたんだぜ

1.0では、既存のHTML、CSS、JavaScriptやpackage.jsonを調べ、モダナイズ候補を見つける仕組みが加わったんだぜ。

Googleによるとガイドは専門家のレビューを受け、ユーザー向けのアクセシビリティガイドは実際の支援技術でもテストしたんだぜ。ガイドなしのエージェントとの比較結果を確認できる公開評価ダッシュボードも用意しているぜ。

さらにChromeだけでなく**Firefox、Edge、MDNの関係者とWebコミュニティ**が協力する、クロスブラウザの取り組みになったんだぜ。

ガイドは**Baseline**の互換性データを使い、ターゲットのブラウザ条件に合わせる設計なんだぜ。プロジェクト側で指定しない場合は、公式導入ガイドによれば**Baseline Widely availableが既定**なんだぜ。

## ただし万能の自動修正ではないんだぜ

正式版でもモダナイズの静的解析は**限定されたパターンから開始**したとGoogleは説明しているんだぜ。すべての古いコードを検出して安全に直すと保証したわけではないんだぜ。

提案された機能が使えるかは対象ブラウザ次第。フォールバックの実装やテスト、レビューは依然として必要なんだぜ。公式の評価もTriva自身の実機比較とは区別するんだぜ。

## だからなんだぜ

AIのコードを良くする方法は、モデルを強くすることだけじゃないんだぜ。

**新しいWeb標準と現場の互換性条件を、その場でエージェントに教える**のも重要な開発技術なんだぜ！

### Sources

- Google Chrome公式発表（2026-11-10）、Modern Web Guidance 1.0 — https://developer.chrome.com/blog/modern-web-guidance-v1
- Google Chrome公式製品ドキュメント — https://developer.chrome.com/docs/modern-web-guidance
- 公式導入ガイド・Baseline互換性 — https://developer.chrome.com/docs/modern-web-guidance/get-started
- 公式ソースリポジトリ — https://github.com/GoogleChrome/modern-web-guidance-src
- エージェント向け配布リポジトリ — https://github.com/GoogleChrome/modern-web-guidance
- 2026-11-12に掲載確認したGitHub TrendingのToday一覧 — https://github.com/trending?since=daily
