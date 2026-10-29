# HTTP2Shell、圧縮データを戻す処理に穴があったんだぜ！

- **掲載日**: 2026-10-25
- **確認日**: 2026-10-25
- **出来事の日付**: 2026-10-21〜23（各プロジェクトの修正・告知）
- **区分**: Tech News / セキュリティ更新
- **対象**: Google Brotli、Node.js、PythonのBrotli関連パッケージ、Bun
- **識別子**: CVE-2026-111497（Node2Shell／HTTP2Shellとして紹介されている脆弱性）

## おいおい、データを小さくする仕組みの中に穴があったんだぜ

Webでは、通信するデータを圧縮して、受け取った側で元に戻すことがあるんだぜ。
その圧縮方式の一つが **Brotli** なんだぜ。

今回、GoogleのBrotliライブラリの **展開処理で、確保した領域の外へメモリを書き込む脆弱性** が公表されたんだぜ。
不正な圧縮データを処理したときに、アプリの停止、メモリ内容の漏えい、遠隔からのコード実行につながる可能性があるんだぜ。

「データを元に戻したら、プログラムの状態まで壊れた」は困るんだぜ！

Node.jsは2026年10月21日のセキュリティリリースで修正を告知したんだぜ。
その後、Pythonの関連パッケージやBunについても、同じCVEの影響と修正が案内されているんだぜ。

## 名前より、共通部品を追うんだぜ

この問題は、Node2ShellやHTTP2Shellという呼び名で紹介されているんだぜ。
ただし、影響を調べるときの目印は **CVE-2026-111497と、利用しているBrotliデコーダー** なんだぜ。

名前に「HTTP2」があるからといって、HTTP/2を使っているかだけで判断できる話ではないんだぜ。
公式告知が示す原因は、圧縮データを展開する共通ライブラリにあるんだぜ。

~~~text
外から届いたBrotli圧縮データなんだぜ
    ↓
アプリやライブラリが展開するんだぜ
    ↓
その中で使うBrotliデコーダーに修正が入っているか確認するんだぜ
~~~

**同じ部品を組み込んでいれば、別の言語のアプリにも影響が広がり得る**んだぜ。
逆に、影響を受けるデコーダーで外部入力を展開する経路があるかも、アプリごとに確認する必要があるんだぜ。

## 修正された版を、利用している製品ごとに確認するんだぜ

2026年10月25日に確認した修正版の例は、次のとおりなんだぜ。

| 対象 | この脆弱性への修正が提供された版 | 確認元 |
| --- | --- | --- |
| Google Brotli | 1.0.10 / 1.1.1 / 1.2.1（各系列） | [公式リリース](https://github.com/google/brotli/releases) |
| Node.js | 22.23.4 / 24.22.1 / 26.12.1（各系列） | [公式セキュリティ告知](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases) |
| Pythonパッケージ `Brotli` | 1.2.1 | [PyPAアドバイザリ](https://github.com/pypa/advisory-database/blob/main/vulns/brotli/PYSEC-2026-4322.yaml) |
| Pythonパッケージ `brotlicffi` | 1.2.1.0 | [PyPAアドバイザリ](https://github.com/pypa/advisory-database/blob/main/vulns/brotlicffi/PYSEC-2026-4323.yaml) |
| Bun | 1.4.4 | [Bun公式アドバイザリ](https://github.com/oven-sh/bun/security/advisories/GHSA-663w-cv8g-jj2p) |

これは修正提供時の版を並べたものなんだぜ。導入時は、利用する系列の最新の対応版と配布元の告知を確認するんだぜ。
古い系列では修正だけが取り込まれることもあるから、番号を大きさだけで一列に比べないんだぜ。

## パソコンのランタイムを更新して終わり、とは限らないんだぜ

Bunの公式告知は、`bun build --compile`で作った実行ファイルにランタイムが組み込まれる点も説明しているんだぜ。
影響を受ける版で作った実行ファイルには、修正済みの版での再ビルドが必要なんだぜ。

**動かしているアプリ、コンテナ、配布した実行ファイルが、どの部品を実際に使うか**を追いかけたいんだぜ。
更新後は必要な再起動や再配布まで確認するんだぜ。

ここで確認したのは脆弱性と修正の告知なんだぜ。
「該当する版を使っていた」という情報だけで、侵害されたと決めつけることはできないんだぜ。
この記事では実際の攻撃や被害の有無は検証していないんだぜ。

## だからなんだぜ

**自分が書いた言語の外側にある、圧縮用の共通部品まで更新対象になる。**

HTTP2Shellのニュースは、依存ライブラリをどこまで組み込んで配っているかを考えさせる話なんだぜ。
「うちはNode.jsを使ってないぜ」で確認を終わらせず、利用製品の公式告知と実際の構成を見ていこうぜ！

この記事は公開資料に基づく解説なんだぜ。実環境の脆弱性検査や修正の適用確認は実施していないんだぜ。

### Sources

- [Node.js：2026年10月21日のセキュリティリリース](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases) — 影響、修正版、外部のBrotliデータを展開する条件
- [Google Brotli公式リリース](https://github.com/google/brotli/releases) — CVE-2026-111497の修正を各系列へ提供
- [PyPA：Brotliのアドバイザリ](https://github.com/pypa/advisory-database/blob/main/vulns/brotli/PYSEC-2026-4322.yaml) — Pythonパッケージの影響と修正版
- [PyPA：brotlicffiのアドバイザリ](https://github.com/pypa/advisory-database/blob/main/vulns/brotlicffi/PYSEC-2026-4323.yaml) — Pythonパッケージの影響と修正版
- [Bun公式アドバイザリ](https://github.com/oven-sh/bun/security/advisories/GHSA-663w-cv8g-jj2p) — 修正版、組み込みランタイムと再ビルドの説明
- [Tanium：HTTP2Shellの解説](https://www.tanium.com/blog/http2shell-find-and-fix-vulnerable-brotli-copies-with-tanium) — HTTP2Shellという呼称の使用例。技術的な影響と修正版は上記の公式資料を参照したんだぜ
