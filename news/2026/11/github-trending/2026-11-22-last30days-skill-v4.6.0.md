# AIに直近30日のネットを調べさせるんだぜ！ last30days-skill v4.6.0がTrending入りだぜ！

- **出来事の日付**: 2026-11-22（v4.6.0リリース）／2026-11-25（GitHub Trendingの記録）
- **執筆日**: 2026-11-26
- **区分**: GitHub Trending / AIエージェント・リサーチ
- **対象**: mvanhorn/last30days-skill
- **ライセンス**: MIT

## AIエージェントに「最近どうなってる？」を調べさせるんだぜ！

GitHubで注目を集めているのが、**mvanhorn/last30days-skill** なんだぜ。

このリポジトリは、AIエージェントに直近30日間の情報を調査させるSkillなんだぜ。Reddit、X、YouTube、Hacker News、Polymarket、Webなど複数の情報源を調べて、最近の反応や話題をまとめることを目指しているんだぜ。

GitHub ExploreのTrending Repository枠にも掲載され、Trendodayが保存した2026年11月25日のWeekly Trending一覧では**5位**に記録されているんだぜ。後者はGitHub公式ランキングそのものではなく、第三者による当日の保存記録だと区別しておくぜ。

## v4.6.0ではSNSの取得方法も改善したんだぜ！

2026年11月22日に公開されたv4.6.0では、次の変更が入ったんだぜ。

- TikTokで、対象として残した投稿の上位コメントを公開コメントAPIから取得
- TikTok・Instagram向けに、Web検索と`yt-dlp`を使うAPIキー不要のフォールバックを追加
- 複数のエージェント実行環境で完成した調査レポートを取り込み、オフラインの文字起こしを回収する仕組みを追加

特定のサービスでデータ取得が難しいときに、別の手段を使えるようにしているんだぜ。

## どのエージェントでも同じように使えるんだぜ？

READMEでは、Claude CodeだけでなくCodex、Cursor、Copilot、Gemini CLIなどのAgent Skills対応環境も案内されているんだぜ。

ただし、何でも完全に無料・無設定で集められるわけではないんだぜ。

- 実行環境にはPython 3.12以降とNode.jsが必要
- 情報源によってはAPIキーやアカウント認証が必要
- v4.6.0のAPIキー不要フォールバックはTikTok・Instagram向けの代替経路で、全サービスの制限をなくすものではない
- SNS側の仕様変更や取得制限によって、情報源ごとの可用性は変わる

つまり、**複数の情報源を束ねる調査Skillであって、全情報を必ず取得できる魔法の検索エンジンではない**んだぜ。

## まとめをそのまま事実だと思わないんだぜ！

このSkillの価値は、検索結果を一つずつ開く前に、複数の場所で何が話題になっているかを横断して把握しやすくすることなんだぜ。

でも、SNSの反応が多いことと、内容が正しいことは別なんだぜ。重要な仕様やリリースを判断するときは、公式ドキュメントやリポジトリのリリースノートまで戻って確認するんだぜ。

## だからなんだぜ

last30days-skillは、AIエージェントに**検索だけでなく、最近のコミュニティの反応を横断して調査する役割**を持たせようとしているんだぜ。

v4.6.0では取得経路も改善され、11月25日のTrending記録でも注目されていたぜ。調査の入口としては面白いけれど、最後の事実確認は一次情報で行うんだぜ！

### Sources

- GitHub repository — https://github.com/mvanhorn/last30days-skill
- v4.6.0 release（2026-11-22）— https://github.com/mvanhorn/last30days-skill/releases/tag/v4.6.0
- Skill specification — https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md
- GitHub Explore — https://github.com/explore/
- Trendoday, GitHub Trending weekly archive（2026-11-25）— https://trendoday.com/github/trending/weekly/2026/11/25
