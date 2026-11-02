# #010 文字ってな、1文字＝1文字コードじゃないんだぜ

プログラムを始めると、

```text
"A" は1文字なんだぜ
"B" も1文字なんだぜ
"あ" も1文字なんだぜ
```

って感覚になるんだぜ。

だから、

> **画面に1文字見えてるなら、コンピュータの中でも1個の文字データなんだぜ。**

と思いがちなんだぜ。

でもな。

**Unicodeの世界では「1文字」って、何を1文字と呼んでるのかで答えが変わるんだぜ。**

### 同じ「é」なのに中身が違うことがあるんだぜ

例えばこの文字なんだぜ。

```text
é
```

こいつは、

```text
U+00E9  LATIN SMALL LETTER E WITH ACUTE
```

という **1個のコードポイント** で表せるんだぜ。

でも同じ見た目を、

```text
U+0065  e
U+0301  COMBINING ACUTE ACCENT
```

という **2個のコードポイント** でも作れるんだぜ。

つまり、

```text
é
```

と見えていても、中身が

```text
[é]
```

の場合と、

```text
[e][ ́]
```

の場合があるんだぜ。

人間の目にはほぼ同じ1文字なんだぜ。

でもコンピュータから見ると、同じ並びとは限らないんだぜ。

だからUnicodeには **正規化（Normalization）** なんて話まで出てくるんだぜ。

### 絵文字になるともっとひどいんだぜ

例えばこれなんだぜ。

```text
👨‍👩‍👧‍👦
```

見た目は **家族の絵文字1個** なんだぜ。

でも中では、

```text
👨
ZWJ
👩
ZWJ
👧
ZWJ
👦
```

という並びなんだぜ。

ZWJは **ZERO WIDTH JOINER** なんだぜ。

ざっくり言えば、

> **この絵文字と次の絵文字をくっつけて1個っぽく表示してくれなんだぜ。**

という文字なんだぜ。

つまり画面では1個でも、内部では複数のUnicodeコードポイントなんだぜ。

Unicode自身も、ZWJ sequenceは内部的には文字の列なのに、利用者からは1個の絵文字のように振る舞うとしてるんだぜ。

### JavaScriptで数えると事故るんだぜ

ここでJavaScriptなんだぜ。

```javascript
"abc".length
```

は、

```text
3
```

なんだぜ。

まあ普通なんだぜ。

でも、

```javascript
"😀".length
```

は、

```text
2
```

なんだぜ。

1個に見えるのに2なんだぜ。

さらに、

```javascript
"👨‍👩‍👧‍👦".length
```

は、

```text
11
```

なんだぜ。

**1文字に見えるものを数えたら11なんだぜ。**

意味が分からないんだぜ。

### JavaScriptのlengthは「文字数」を数えてないんだぜ

理由はJavaScriptの文字列が **UTF-16** で扱われるからなんだぜ。

JavaScriptの `.length` が数えてるのは、

> **人間が見ている文字数なんだぜ**

じゃなくて、

> **UTF-16のコード単位が何個あるんだぜ**

なんだぜ。

だから、

```text
"A"
```

みたいな文字なら1コード単位なんだぜ。

でも、

```text
😀
```

みたいなBMPの外にある文字は、UTF-16では **サロゲートペア** という2コード単位になるんだぜ。

なので、

```javascript
"😀".length === 2
```

になるんだぜ。

### じゃあArray.fromなら解決なんだぜ？

そこで、

```javascript
Array.from("😀").length
```

なら、

```text
1
```

になるんだぜ。

いい感じなんだぜ。

でも、

```javascript
Array.from("👨‍👩‍👧‍👦").length
```

は、

```text
7
```

なんだぜ。

まだ1じゃないんだぜ。

なぜなら `Array.from` はUnicodeの **コードポイント** 単位ではかなりマシに扱えるけど、

> **人間が見て1文字なんだぜ**

という単位までは数えてないからなんだぜ。

家族絵文字は、

```text
👨 + ZWJ + 👩 + ZWJ + 👧 + ZWJ + 👦
```

の7コードポイントなんだぜ。

### 人間が思う「1文字」に近いものには名前があるんだぜ

Unicodeでは、人間がだいたい「これで1文字なんだぜ」と感じるまとまりに近い概念として **grapheme cluster** があるんだぜ。

ざっくり整理すると、

```text
glyph
    画面に描かれる形なんだぜ

grapheme cluster
    人間が1文字っぽく感じる単位なんだぜ

code point
    Unicode上の番号なんだぜ

code unit
    UTF-8やUTF-16みたいな符号化方式での単位なんだぜ
```

なんだぜ。

こいつらは全部同じじゃないんだぜ。

ここが文字列処理の沼の入口なんだぜ。

### JavaScriptでも「見た目の文字」で分けられるんだぜ

今のJavaScriptなら `Intl.Segmenter` を使ってgrapheme単位に分割できるんだぜ。

```javascript
const segmenter = new Intl.Segmenter("ja", {
    granularity: "grapheme"
});

const chars = [...segmenter.segment("👨‍👩‍👧‍👦")];

console.log(chars.length);
```

なら、

```text
1
```

になるんだぜ。

ようやく人間の感覚に近づいたんだぜ。

### 国旗も1文字じゃないんだぜ

例えば、

```text
🇯🇵
```

なんだぜ。

見た目は日本国旗1個なんだぜ。

でもこれは **REGIONAL INDICATOR SYMBOL LETTER J** と **REGIONAL INDICATOR SYMBOL LETTER P** の2コードポイントの組み合わせなんだぜ。

JavaScriptではそれぞれがサロゲートペアになるので、

```javascript
"🇯🇵".length
```

は、

```text
4
```

なんだぜ。

旗1個なのに4なんだぜ。

### だから文字数制限は意外と難しいんだぜ

例えばSNSを作って、

> **名前は10文字までなんだぜ。**

としたいんだぜ。

じゃあ何を10個までにするんだぜ。

```text
10バイトなんだぜ？
10 UTF-16コード単位なんだぜ？
10コードポイントなんだぜ？
10 grapheme clusterなんだぜ？
```

全部結果が違う可能性があるんだぜ。

「文字数を数える」って聞くと小学生でも分かりそうな処理なんだぜ。

でもコンピュータで真面目にやると、

**そもそも文字とは何なんだぜ**

から始まるんだぜ。

### だからなんだぜ

画面に、

```text
👨‍👩‍👧‍👦
```

が1個見えてるんだぜ。

人間は、

> **1文字なんだぜ。**

と思うんだぜ。

Unicodeは、

> **複数のコードポイントからなるgrapheme clusterなんだぜ。**

と言うんだぜ。

JavaScriptの `.length` は、

> **11なんだぜ。**

と言うんだぜ。

全員違うんだぜ。

だから、

> **1文字＝1文字コードなんだぜ**

は、ASCIIくらいの世界では気持ちよく成り立つんだけど、

**Unicodeの世界に出た瞬間、だいたい壊れるんだぜ。**

### Sources

- Unicode Consortium, *FAQ — Characters and Combining Marks*  
  https://www.unicode.org/faq/char_combmark.html
- Unicode Standard Annex #29, *Unicode Text Segmentation*  
  https://www.unicode.org/reports/tr29/
- Unicode Technical Standard #51, *Unicode Emoji*  
  https://unicode.org/reports/tr51/
- Unicode Consortium, *Recommended Emoji ZWJ Sequences*  
  https://unicode.org/emoji/charts/emoji-zwj-sequences.html
- MDN, *String: length*  
  https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String/length
- MDN, *Intl.Segmenter*  
  https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl/Segmenter
