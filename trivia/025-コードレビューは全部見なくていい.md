# #025 コードレビューってな、一人で全部を見る必要はないんだぜ

コードレビューって聞くと、

```text
diffを最初から最後まで読むんだぜ
全部のバグを見つけるんだぜ
全部の設計を理解するんだぜ
全部のテストを確認するんだぜ
全部のセキュリティ問題を見るんだぜ
```

みたいな仕事に見えるんだぜ。

でもな。

**一人のレビュアーが、全部を同じ深さで見る必要はないんだぜ。**

むしろ大きい変更ほど、

> **誰が、どこを、何の観点で見るんだぜ？**

を分けた方がいいんだぜ。

## レビューには「観点」があるんだぜ

同じPRでも、見るものは一個じゃないんだぜ。

```text
設計なんだぜ
機能なんだぜ
性能なんだぜ
セキュリティなんだぜ
DBなんだぜ
並行処理なんだぜ
UIなんだぜ
アクセシビリティなんだぜ
テストなんだぜ
運用なんだぜ
```

なんだぜ。

Googleのcode review guideでも、

```text
design
functionality
complexity
tests
naming
comments
style
documentation
```

みたいに、レビュー項目を分けてるんだぜ。

つまり、

> **コードレビュー = 誤字脱字を探す作業なんだぜ。**

でもないし、

> **全部の観点を一人で完全に見る作業なんだぜ。**

でもないんだぜ。

## セキュリティはセキュリティの人に見てもらえばいいんだぜ

例えば認証まわりの変更なんだぜ。

普通のレビュアーが、

```text
コードの構造なんだぜ
変数名なんだぜ
テストなんだぜ
```

を見るのはできるんだぜ。

でも、

```text
権限昇格できないんだぜ？
CSRF大丈夫なんだぜ？
token漏れないんだぜ？
cryptoの使い方正しいんだぜ？
```

まで自信がないかもしれないんだぜ。

そのときは、

> **自分が全部判断するんだぜ。**

じゃないんだぜ。

> **この部分はsecurityに詳しい人も見てくれなんだぜ。**

でいいんだぜ。

Googleのguideも、privacy・security・concurrency・accessibility・internationalizationみたいな専門領域では、**その部分をレビューできる人を追加するんだぜ**としてるんだぜ。

## GitHubのCODEOWNERSも「全部を一人で見ない」仕組みなんだぜ

GitHubには **CODEOWNERS** があるんだぜ。

例えば、

```text
/backend/db/       @database-team
/security/         @security-team
/frontend/         @frontend-team
```

みたいにしておくんだぜ。

するとその領域が変わったとき、対応する人やチームへレビューを飛ばせるんだぜ。

GitHub自身も、Code ownersは、

> **そのファイルやディレクトリに詳しい人へレビューをrouteするための仕組みなんだぜ。**

としてるんだぜ。

つまり、

**PR全体を一人の万能レビュアーへ投げる必要はないんだぜ。**

## 同じPRを「ファイルごと」に分担していいんだぜ

例えばPRが、

```text
APIなんだぜ
DB migrationなんだぜ
frontendなんだぜ
Terraformなんだぜ
```

を触ってるんだぜ。

一人で全部詳しい人なんて、そうそういないんだぜ。

だから、

```text
API
    backend担当が見るんだぜ

migration
    DB担当が見るんだぜ

frontend
    frontend担当が見るんだぜ

Terraform
    infra担当が見るんだぜ
```

でいいんだぜ。

Googleのcode review guideにも、

> **複数レビュアーで、特定ファイルだけ・特定観点だけを見るのは普通なんだぜ。**

という例外が明記されてるんだぜ。

その代わり、

> **自分はどこを見たんだぜ。**

を明示するんだぜ。

### 「このPR見ました」じゃなくて「ここを見ました」なんだぜ

例えば、

```text
設計とAPI interfaceを見たんだぜ。
DB migrationは見てないんだぜ。
securityは別の人にお願いしてるんだぜ。
```

と書くんだぜ。

これなら、

> **誰かが見たと思って誰も見てなかったんだぜ。**

を防げるんだぜ。

**レビューの分担には、責任範囲の明示が必要なんだぜ。**

## styleは人間が毎回見る必要ないんだぜ

例えば、

```text
空白なんだぜ
import順なんだぜ
formatなんだぜ
lintなんだぜ
型チェックなんだぜ
```

なんかは、かなり機械で見られるんだぜ。

```text
formatterなんだぜ
linterなんだぜ
type checkerなんだぜ
static analysisなんだぜ
CIなんだぜ
```

に任せればいいんだぜ。

GitHubも、PRを標準化する方法として、

```text
status checks
automated tools
code scanning
dependency review
```

みたいなものを使えるとしてるんだぜ。

人間が、

> **ここスペース一個足りないんだぜ。**

を毎回探すより、

**機械に見られるものは機械に見せた方がいいんだぜ。**

## generated codeを一行ずつ読む意味は薄いことがあるんだぜ

例えば、

```text
protobuf生成コードなんだぜ
OpenAPIから生成したclientなんだぜ
ORM生成コードなんだぜ
大量のfixtureなんだぜ
巨大なデータtableなんだぜ
```

がPRに入ってるんだぜ。

これを人間が、

> **1行目から50000行目まで全部精読するんだぜ。**

は効率が悪いんだぜ。

Googleのguideも、generated codeやdata file、大きなdata structureは、場合によってはscanで済ませられるとしてるんだぜ。

この場合に見るべきなのは、

```text
生成元が正しいんだぜ？
generator versionは意図通りなんだぜ？
差分量は妥当なんだぜ？
再生成できるんだぜ？
想定外のfileが混ざってないんだぜ？
```

だったりするんだぜ。

**生成物そのものより、生成方法をレビューした方が価値が高いことがあるんだぜ。**

## 「全部見る」より「危ないところを深く見る」なんだぜ

コードは全部同じリスクじゃないんだぜ。

例えば、

```text
READMEの誤字修正なんだぜ
```

と、

```text
認証処理を書き換えるんだぜ
```

を同じ密度でレビューする必要はないんだぜ。

また、

```text
単純なDTO追加なんだぜ
```

と、

```text
lockを使う並行処理なんだぜ
```

も違うんだぜ。

Googleも、**コードによって必要なscrutinyの深さは違うんだぜ**としてるんだぜ。

だから、

```text
認証なんだぜ
権限なんだぜ
お金なんだぜ
データ削除なんだぜ
migrationなんだぜ
concurrencyなんだぜ
公開APIなんだぜ
```

みたいなところは深く見るんだぜ。

一方、

```text
機械生成なんだぜ
単純なrenameなんだぜ
formatter差分なんだぜ
```

は、確認の仕方を変えていいんだぜ。

**レビューは行数均等配分じゃなくて、リスク配分なんだぜ。**

## ただし「誰も全部見なくていい」ではないんだぜ

ここは重要なんだぜ。

> **じゃあ誰もコードをちゃんと読まなくていいんだぜ。**

ではないんだぜ。

Googleのguideも、一般論としては、

> **自分がreview担当になったhuman-written codeは一行ずつ理解するんだぜ。**

としてるんだぜ。

つまり、

```text
一人で全部見る必要はないんだぜ
```

と、

```text
誰も全部見なくていいんだぜ
```

は全然違うんだぜ。

人間が書いた重要なコードは、

**チーム全体としては誰かが理解している状態**

にするんだぜ。

## PRがでかすぎるなら、レビュー方法よりPRを直すんだぜ

もし、

> **全部見るの無理なんだぜ。**

となった理由が、

```text
変更が5000行なんだぜ
機能追加とrefactorが混ざってるんだぜ
renameとformatと仕様変更が一緒なんだぜ
```

なら、

> **レビュアーが頑張るんだぜ。**

じゃなくて、

> **PRを分けるんだぜ。**

なんだぜ。

GoogleもGitHubも、小さく焦点の合った変更の方がレビューしやすく、問題も見つけやすいとしてるんだぜ。

例えば、

```text
PR1
    renameだけなんだぜ

PR2
    refactorだけなんだぜ

PR3
    behavior changeなんだぜ
```

に分ければ、

> **何をレビューすればいいんだぜ？**

が一気に分かりやすくなるんだぜ。

## レビューは「網羅試験」じゃないんだぜ

コードレビューで、

```text
全バグを発見するんだぜ
全仕様を確認するんだぜ
全性能問題を見つけるんだぜ
全security holeを見つけるんだぜ
```

なんて無理なんだぜ。

だから、

```text
unit testなんだぜ
integration testなんだぜ
static analysisなんだぜ
fuzzingなんだぜ
benchmarkなんだぜ
security scanなんだぜ
monitoringなんだぜ
```

も使うんだぜ。

コードレビューは品質保証の**一工程**なんだぜ。

全部をレビューへ押し込むんじゃないんだぜ。

**テストが得意なことはテストに任せるんだぜ。**
**静的解析が得意なことは静的解析に任せるんだぜ。**
**人間は設計意図や危険な前提を見るんだぜ。**

## だからなんだぜ

コードレビューで大事なのは、

> **全部見たんだぜ。**

という達成感じゃないんだぜ。

大事なのは、

```text
何が危ない変更なんだぜ
誰がそこに詳しいんだぜ
機械で確認できるものは何なんだぜ
人間が考えるべきところは何なんだぜ
誰がどこまで確認したんだぜ
```

なんだぜ。

**コードレビューは、全行を一人で読破する競技じゃないんだぜ。**

**必要なところを、必要な人と必要な道具で見るんだぜ。**

### Sources

- Google Engineering Practices, *What to look for in a code review*  
  https://google.github.io/eng-practices/review/reviewer/looking-for.html
- Google Engineering Practices, *Small CLs*  
  https://google.github.io/eng-practices/review/developer/small-cls.html
- GitHub Docs, *Helping others review your changes*  
  https://docs.github.com/en/pull-requests/concepts/helping-others-review-your-changes
- GitHub Docs, *About code owners*  
  https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
