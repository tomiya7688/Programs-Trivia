# #014 Pythonを作った人は、もうPythonの王様じゃないんだぜ

Pythonを作ったのは **Guido van Rossum** なんだぜ。

1989年のクリスマス休暇ごろに作り始めて、その後ずっとPythonの中心人物だったんだぜ。

しかもただの作者じゃなかったんだぜ。

長いあいだGuidoには、

```text
BDFL
```

という肩書きがあったんだぜ。

**Benevolent Dictator For Life**。

日本語にすると、

> **慈悲深き終身独裁者なんだぜ。**

くらいの意味なんだぜ。

名前が強すぎるんだぜ。

### 本当に「最後はGuidoが決める」だったんだぜ

もちろん普段のPython開発は、たくさんのcore developerが議論して進めていたんだぜ。

でも言語仕様みたいな大きな話で意見が割れたとき、

> **最後に誰が決めるんだぜ？**

となったら、Guidoが最終判断をする立場だったんだぜ。

PythonのPEP、つまり言語の改善提案でも、

```text
提案するんだぜ
↓
みんなで議論するんだぜ
↓
最後の判断が必要ならGuidoなんだぜ
```

という時代が長く続いたんだぜ。

つまり、

> **Pythonを作った人が、そのままPythonの最終決定者だったんだぜ。**

なんだぜ。

### でも2018年に突然やめたんだぜ

2018年7月12日。

PEP 572、いわゆる **walrus operator `:=`** をめぐるかなり激しい議論のあと、GuidoはPythonのcommitters mailing listに投稿したんだぜ。

内容をものすごく雑に言うと、

> **もう最終決定者やるの疲れたんだぜ。**

なんだぜ。

実際には、

```text
I would like to remove myself entirely from the decision process.
```

と書いてるんだぜ。

さらに、

```text
I'm basically giving myself a permanent vacation from being BDFL.
```

なんだぜ。

つまり、

> **BDFLから永久休暇をもらうんだぜ。**

と宣言したんだぜ。

しかも後継者を指名しなかったんだぜ。

### 「じゃあ次のPython王は誰なんだぜ？」にはならなかったんだぜ

ここが面白いんだぜ。

Guidoが辞めたなら、

> **次のBDFLを決めるんだぜ。**

になりそうなんだぜ。

でもPythonはそうしなかったんだぜ。

コミュニティで新しい統治方法を議論して、最終的に **Steering Council** 方式になったんだぜ。

PEP 13では、Pythonの運営は **5人のSteering Council** が担うことになってるんだぜ。

つまり、

```text
昔
    最終判断 → Guidoなんだぜ

今
    最終判断 → 選挙で選ばれた5人のCouncilなんだぜ
```

なんだぜ。

Pythonは、

> **作者がずっと支配する言語なんだぜ。**

から、

> **コミュニティが制度で運営する言語なんだぜ。**

に変わったんだぜ。

### じゃあGuidoはもうPythonに関わってないんだぜ？

……と思うんだぜ。

でも、ここで話がもう一段ひっくり返るんだぜ。

**完全には離れてないんだぜ。**

2018年の辞任メッセージでも、

> **ordinary core devとしては残るんだぜ。**

という趣旨を書いてたんだぜ。

その後もPython高速化の仕事に関わったし、CPythonへの変更も続けてるんだぜ。

そして2026年8月にも、Guido本人のGitHubアカウントからCPythonへ、

```text
Runtime changes for PEP 718 -- generic function subscription
```

というPRを出してるんだぜ。

つまり、

> **Pythonを作った人は、もうPythonに関わってないんだぜ。**

は正確には違うんだぜ。

正しくは、

> **Pythonを作った人は、もうPythonの最終決定者じゃないんだぜ。**

なんだぜ。

### 作者なのに、自分の言語を自由に決められないんだぜ

これ、よく考えるとかなり面白いんだぜ。

PythonはGuidoが始めた言語なんだぜ。

でも今Guidoが、

> **Pythonにこの機能を入れるんだぜ。**

と言っても、

```text
作者だから採用なんだぜ
```

にはならないんだぜ。

他の提案と同じように、

```text
PEPを書くんだぜ
議論するんだぜ
レビューされるんだぜ
必要ならSteering Councilが判断するんだぜ
```

なんだぜ。

**作者よりプロジェクトの制度の方が上なんだぜ。**

オープンソースが十分に大きくなると、

> **誰が作ったか**

より、

> **どうやってみんなで決めるか**

の方が重要になるんだぜ。

### だからなんだぜ

Pythonを作ったGuido van Rossumは、長いあいだ **BDFL** だったんだぜ。

文字どおり、

> **Pythonの慈悲深き終身独裁者なんだぜ。**

だったんだぜ。

でも2018年に自分からその座を降りたんだぜ。

今のPythonは、作者ひとりが決める言語じゃないんだぜ。

そして一番面白いのは、

**王様を辞めた本人が、今も普通の開発者としてPythonにコードを持ってくることがあるんだぜ。**

なんだぜ。

**Pythonを作った人は、Pythonから消えたんじゃないんだぜ。**

**Pythonの王様をやめたんだぜ。**

### Sources

- Guido van Rossum, *Transfer of power*, python-committers, 2018-07-12  
  https://mail.python.org/archives/list/python-committers@python.org/thread/GQONAGWBBFRHVRUPU7RNBM75MHKGUFJN/
- PEP 13, *Python Language Governance*  
  https://peps.python.org/pep-0013/
- CPython PR #156201, opened by Guido van Rossum, 2026-08-21  
  https://github.com/python/cpython/pull/156201
