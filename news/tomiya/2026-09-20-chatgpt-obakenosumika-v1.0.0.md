# おばけの住処 GPT、Ver1.0.0が公開されたんだぜ！

- **掲載日**: 2026-10-15
- **出来事の日付**: 2026-09-20
- **区分**: Tomiya News / 自作プロジェクトのリリース
- **対象**: [tomiya7688/chatgpt-obakenosumika](https://github.com/tomiya7688/chatgpt-obakenosumika)
- **リリース**: [Ver1.0.0](https://github.com/tomiya7688/chatgpt-obakenosumika/releases/tag/Ver1.0.0)

## まるとかどかが、AIとの会話にやってきたんだぜ！

Tomiyaが公開している **「おばけの住処 — ChatGPT / Grok」** に、2026年9月20日付で **Ver1.0.0** の正式リリースがあるんだぜ。

登場するのは、地下の住処で暮らす「まる」と「かどか」という2匹のおばけなんだぜ。

といっても、ゲームや独立したチャットアプリの完成を発表したわけじゃないんだぜ。

**AIに2匹を演じてもらうための、世界観・キャラクター設定・会話指示をまとめたプロジェクト**なんだぜ。

## 設定とプロンプトを分けてあるんだぜ

ファイルの役割がきちんと分かれているのが特徴なんだぜ。

- **`WORLD.md`** — おばけの住処がどんな場所なのかを定義するんだぜ
- **`CHARACTERS.md`** — まるとかどかの性格、口調、関係を定義するんだぜ
- **`chatgpt/PROMPT.md`** — ChatGPTでの演じ方を指示するんだぜ
- **`grok/PROMPT.md`** — Grokでの演じ方を指示するんだぜ

ここが面白いんだぜ。

~~~text
世界設定を直すんだぜ
    ↓
両方のAIで同じ設定を参照できるんだぜ
    ↓
AIごとの差は演技用の指示で調整するんだぜ
~~~

という設計なんだぜ。

AIに世界観を全部丸投げするのではなく、**設定の正本をGitHubに置いておく**という考え方なんだぜ。

## 最新版と固定版を区別できるんだぜ

READMEには、指定がなければ`main`の最新設定を正本として使うこと、バージョンを指定したら対応するタグを参照することが説明されているんだぜ。

つまり、設定の更新を続けながら、「Ver1.0.0の設定で会話して」と固定する使い方もできるんだぜ。

実際、Ver1.0.0の公開後も`main`で設定の修正が続いているんだぜ。
**1.0.0を公開したことと、以後まったく変更しないことは別**なんだぜ。

## 素材のライセンスは別リポジトリが正本なんだぜ

キャラクター画像や関連素材のライセンスは、[Obake_Lisense](https://github.com/tomiya7688/Obake_Lisense) が正本なんだぜ。
設定の正本とライセンスの正本を混同しないようにするんだぜ。

## だからなんだぜ

**オリジナルキャラクターをAIに演じさせるなら、設定の管理と演じ方の指示を分けると扱いやすいんだぜ。**

しかも今回は、Tomiya自身のプロジェクトのニュースなんだぜ。
Programs-Triviaでは今後も、Tomiya製の公開・リリース・大きな更新を、外部ニュースと区別して紹介していくんだぜ！

### Sources

- GitHub Release, *Ver1.0.0* — https://github.com/tomiya7688/chatgpt-obakenosumika/releases/tag/Ver1.0.0
- プロジェクトREADME — https://github.com/tomiya7688/chatgpt-obakenosumika/blob/main/README.md
- ChatGPT用プロンプト — https://github.com/tomiya7688/chatgpt-obakenosumika/blob/main/chatgpt/PROMPT.md
- ライセンスの正本 — https://github.com/tomiya7688/Obake_Lisense
