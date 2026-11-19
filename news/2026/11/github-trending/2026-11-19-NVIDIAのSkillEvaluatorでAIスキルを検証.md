# AIスキルにもCIが必要なんだぜ！ NVIDIA SkillEvaluator 1.0.1がTrendingに来たぜ！

- **掲載日（執筆日）**: 2026-11-19
- **出来事の日付**: 2026-11-17（v1.0.0リリース）、2026-11-18（v1.0.1リリース）
- **Trending確認日**: 2026-11-19（GitHub公式Trending「Today」のリポジトリ一覧）
- **区分**: GitHub Trending
- **対象**: [NVIDIA/SkillEvaluator](https://github.com/NVIDIA/SkillEvaluator) v1.0.1
- **公開状態**: v1.0.1が正式リリース。Apache-2.0ライセンス

## 何が起きたんだぜ？

おいおい、AIの「スキル」を追加するのは簡単なのに、**そのスキルが本当に読み込まれたのか、答えを良くしたのか**を確かめていなかったんだぜ！？

NVIDIAの**SkillEvaluator**は、AIエージェントのスキルやプラグインを検査し、実行結果を比べるためのツールなんだぜ。

2026年11月19日にGitHub公式Trending（Today）への掲載を確認したんだぜ。Star数やランキングは日々変動するから、順位の固定的な断定はしないぜ。

リリース履歴も大事なんだぜ。11月17日にv1.0.0が公開されたけど、そのリリースは**v1.0.1に置き換え済み**と明記されているぜ。**PythonパッケージとCI配布物まで揃った1.0系の完成版は、11月18日公開のv1.0.1**なんだぜ。

## 何を検査するんだぜ？

大きく分けると3段階なんだぜ。

| 段階 | 検査すること | 主な条件 |
| --- | --- | --- |
| **Tier 1** | 品質・構造・セキュリティ・ハーネス互換性 | 静的検査はキー不要。全検査には外部スキャナの準備が必要 |
| **Tier 2** | スキルの重複や意味的な類似性 | 埋め込み・チャット用モデルへのアクセスが必要 |
| **Tier 3** | スキルがある場合とない場合のエージェント実行を比較 | 対応エージェント、モデル接続、サンドボックス（標準はDocker）が必要 |

「定義ファイルは正しいけどエージェントが読み込まない！」という事故も狙い撃ちできるんだぜ。

**HarnessCheck**はClaude Code、Codex、Copilot CLI、Hermes、OpenClaw、OpenCodeなどについて、スキルの検出先や記述長、呼び出し設定を検査するんだぜ。

```bash
# Python 3.12～3.14で導入するんだぜ
python -m pip install --upgrade skillevaluator

# モデルのAPIキーなしで品質確認するんだぜ
skillevaluator quality-check ./my-skill

# スキルが各ハーネスに読み込まれるか調べるんだぜ
skillevaluator harness-check ./my-skill

# 対応スキャナを整えてTier 1だけを検証するんだぜ
skillevaluator validate ./my-skill --tiers 1
```

## GitHubのPRで落とせるのが面白いんだぜ

`skillevaluator ci init` で、リポジトリへのCI導入を案内する仕組みもあるんだぜ。品質検査やセキュリティ検査をコードと同じようにPRのチェックへ組み込めるんだぜ。

v1.0.1では互換性検査の強化に加え、NeMo Gym実行基盤への対応、JUnitレポート、ローカルのpre-commitチェックやPRコメント用ワークフローも拡充されたんだぜ。

## まだ注意点もあるんだぜ

**「キー不要」と「全機能無料・準備不要」は違う**んだぜ。基本の品質チェックはモデルキーなしで使えるけど、全Tierの実行にはモデルプロバイダ、外部スキャナ、Dockerなどの設定が必要なんだぜ。利用サービスによって実行費用も発生するぜ。

品質スコアが高くても、実際のエージェント実行が安全で有益だと確定するわけではないんだぜ。Tier 3の評価用データにもカバレッジの限界があるし、HarnessCheckは標準では助言的な検査なんだぜ。

この記事は**公式リポジトリ・公式リリース・Trendingを確認した紹介**であって、Trivaが実際にスキルを採点したという報告ではないんだぜ。

## だからなんだぜ

AIエージェントに渡す指示だって、ソースコードと同じように**仕様・互換性・セキュリティ・振る舞い**を検証する時代なんだぜ。スキルを増やす前に、そのスキルが本当に役立つかを測るんだぜ！

### Sources

- [GitHub公式Trending（確認日: 2026-11-19）](https://github.com/trending) — 当日の注目リポジトリ掲載を確認したんだぜ
- [NVIDIA/SkillEvaluator 公式リポジトリ](https://github.com/NVIDIA/SkillEvaluator) — インストール・検査方式・制限を確認したぜ
- [NVIDIA公式リリース v1.0.1（2026-11-18）](https://github.com/NVIDIA/SkillEvaluator/releases/tag/v1.0.1) — 1.0系の完成版を確認したんだぜ
- [NVIDIA公式リリース v1.0.0（置き換え済み）](https://github.com/NVIDIA/SkillEvaluator/releases/tag/v1.0.0) — 公開経緯を確認したぜ
