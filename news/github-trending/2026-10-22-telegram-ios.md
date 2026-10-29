# TelegramのiOSアプリって、中身もビルド手順も読めるんだぜ！

- **掲載日**: 2026-10-22
- **確認日**: 2026-10-22
- **区分**: GitHub Trending / 注目リポジトリ
- **対象**: [TelegramMessenger/Telegram-iOS](https://github.com/TelegramMessenger/Telegram-iOS)
- **注目の確認元**: GitHub TrendingのSwift一覧

## 普段使うアプリの作り方を読めるんだぜ

GitHubのSwift注目一覧に、**TelegramのiOSアプリの公式リポジトリ**が掲載されていたんだぜ。

Telegramの公式サイトも、iPhone・iPad向けアプリのソースコードを案内しているんだぜ。
今回は、公開されているアプリの中身やビルドの仕組みを読む題材として紹介するんだぜ。

## 自分でビルドする手順もあるんだぜ

公式READMEには、ソースの取得、Xcodeの準備、設定ファイルの編集、Xcodeプロジェクトの生成手順が書かれているんだぜ。

手元の開発環境でアプリを組み立てるまでの準備を追えるんだぜ。
ただし、自分のアプリとして作るときには、独自の`api_id`や署名まわりの設定が必要なんだぜ。

READMEには、非公式アプリであることの表示、ロゴ、ソース公開などの案内もあるから、変更して配布したいときは確認するんだぜ。

## 配布されているアプリとの関係まで確かめるんだぜ

ここが面白いんだぜ。

Telegramは **再現可能ビルド（reproducible builds）** の検証手順も公開しているんだぜ。

公開されたソースからアプリを組み立て、配布されているアプリと照合する取り組みなんだぜ。
**読めるソースコードと、手元に届くアプリの対応を確かめようとしている**んだぜ。

公式のiOS向け手順では、安定したビルド環境を用意するために仮想マシンを使い、対象のバージョンや開発ツールを合わせる流れが説明されているんだぜ。

## 検証できる範囲も読むんだぜ

公式ガイドは、検証手順を実験的なものとしているんだぜ。
比較結果の例にも、暗号化された拡張機能や、App Store側で処理された一部のリソースなど、検証対象外になる項目が載っているんだぜ。

比較結果を読むときは、**一致した範囲と、確認できなかった範囲**まで見る必要があるんだぜ。

## だからなんだぜ

**アプリのソースを読むことから、配布されるまでの工程を調べることへ。**

実際のアプリを題材に、ビルドと検証の仕組みを学べるリポジトリなんだぜ！

この記事は公式資料に基づく紹介なんだぜ。ビルドや配布アプリとの照合は実施していないんだぜ。
Trendingは変動するため、順位やStar数は記録していないんだぜ。

### Sources

- [Telegram-iOS公式README](https://github.com/TelegramMessenger/Telegram-iOS) — ビルド手順と独自アプリ作成時の案内
- [Telegram Applications](https://telegram.org/apps) — 公式アプリとソースコードの案内
- [Reproducible Builds for iOS and Android](https://core.telegram.org/reproducible-builds) — 再現可能ビルド、iOSの検証手順と対象外項目
- [GitHub Trending — Swift](https://github.com/trending/swift) — Swift一覧での掲載を確認
