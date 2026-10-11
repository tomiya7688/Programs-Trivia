# #022 Pythonはスクリプト言語だから一般配布できない、なんてことはないんだぜ

Pythonでアプリを作る話をすると、

> **でもPythonってスクリプト言語なんだぜ？**
>
> **相手のPCにPythonが入ってないと動かないんだぜ？**
>
> **一般ユーザー向けには配れないんだぜ？**

みたいな話が出ることがあるんだぜ。

でもな。

**そんなことはないんだぜ。**

Pythonで書いたアプリを、Pythonを入れてない人へ配る方法は普通にあるんだぜ。

## そもそも「スクリプト言語」と「配布できるか」は別の話なんだぜ

ここが一番大事なんだぜ。

```text
スクリプト言語なんだぜ
```

という分類と、

```text
一般ユーザーへ配布できるんだぜ
```

は別の話なんだぜ。

Pythonのコードを実行するのにPython処理系が必要なら、

> **処理系も一緒に配ればいいんだぜ。**

なんだぜ。

前に、

> **Pythonはインタプリタ言語……だけじゃないんだぜ。**

って話をしたんだぜ。

CPythonは、

```text
Pythonソース
    ↓
bytecodeへcompileするんだぜ
    ↓
Python VMが実行するんだぜ
```

なんだぜ。

だったら配布時に、

```text
自分のコードなんだぜ
Python本体なんだぜ
必要なライブラリなんだぜ
DLLや共有ライブラリなんだぜ
データファイルなんだぜ
```

をまとめて持っていけばいいんだぜ。

## PyInstallerは本当にPython本体まで持っていくんだぜ

代表的なのが **PyInstaller** なんだぜ。

PyInstallerの公式ドキュメントには、そのまま、

> **Python applicationと依存物をbundleして、利用者はPython interpreterやmoduleをinstallしなくても実行できるんだぜ。**

という説明があるんだぜ。

しかも内部的には、

```text
自分のPythonコードなんだぜ
importしたmoduleなんだぜ
必要なnative libraryなんだぜ
active Python interpreterなんだぜ
```

まで集めるんだぜ。

つまり、

> **相手にPythonがないんだぜ。**

への答えは、

> **じゃあPythonも同梱するんだぜ。**

なんだぜ。

かなり力技なんだぜ。

でも普通に成立するんだぜ。

### one-folderでもone-fileでも配れるんだぜ

例えば、

```bash
pyinstaller app.py
```

なら、配布用のfolderを作れるんだぜ。

さらに、

```bash
pyinstaller --onefile app.py
```

なら、一つの実行ファイルにまとめることもできるんだぜ。

GUIアプリなら、

```bash
pyinstaller --onefile --windowed app.py
```

みたいな形にもできるんだぜ。

ユーザーから見れば、

```text
app.exeをダブルクリックするんだぜ
```

なんだぜ。

中でPythonが動いてるかなんて、普通の利用者は気にしないんだぜ。

## Nuitkaなら「Pythonをcompileして配る」方向もあるんだぜ

もう一つ有名なのが **Nuitka** なんだぜ。

NuitkaはPython compilerなんだぜ。

例えば、

```bash
python -m nuitka --mode=standalone app.py
```

とすると、他のPCへ持っていけるstandalone distributionを作れるんだぜ。

さらに、

```bash
python -m nuitka --mode=onefile app.py
```

なら、一つの実行ファイルとして配ることもできるんだぜ。

つまりPythonの配布には、

```text
処理系ごとbundleするんだぜ
compileしてstandalone化するんだぜ
packageとして配るんだぜ
```

みたいに、いくつも方法があるんだぜ。

## Pythonには「Pythonが入ってる人向け」の配布方法もあるんだぜ

一方で、相手にPythonが入ってる前提なら、もっと軽い方法もあるんだぜ。

Python標準ライブラリには **zipapp** があるんだぜ。

```bash
python -m zipapp myapp -m "myapp:main"
```

みたいにすると、

```text
myapp.pyz
```

という実行可能なPython zip archiveを作れるんだぜ。

これは、

```bash
python myapp.pyz
```

で実行できるんだぜ。

つまり配布方法にも、

```text
相手にPythonがある前提なんだぜ
Pythonごと持っていくんだぜ
standalone binaryにするんだぜ
```

という違いがあるんだぜ。

**「Pythonアプリの配布方法」は一つじゃないんだぜ。**

## pipで配るのも立派な一般配布なんだぜ

Python開発者向けなら、

```bash
pip install my-package
```

で配るのも普通なんだぜ。

PyPIにはPython packageを公開できるんだぜ。

```text
libraryなんだぜ
CLI toolなんだぜ
frameworkなんだぜ
applicationなんだぜ
```

をpackageとして配るんだぜ。

これは、

> **一般ユーザーがexeをダブルクリックする配布**

とは違うんだぜ。

でもソフトウェア配布としては普通に成立してるんだぜ。

つまり「配布」という言葉自体にも、

```text
開発者向けpackageなんだぜ
社内toolなんだぜ
CLIなんだぜ
desktop appなんだぜ
serverなんだぜ
```

で最適な方法が違うんだぜ。

## もちろんCで作ったexeと全く同じではないんだぜ

ここは注意なんだぜ。

PyInstallerで一つのexeにしたからといって、

> **PythonがCになったんだぜ。**

ではないんだぜ。

PyInstallerはPython interpreterや必要なmoduleをbundleしてるんだぜ。

だから普通の小さなnative programと比べると、

```text
配布サイズが大きくなりやすいんだぜ
起動時に展開が入ることがあるんだぜ
依存packageの扱いが必要なんだぜ
native extensionも一緒に考えるんだぜ
```

みたいな違いがあるんだぜ。

Nuitkaでもstandalone配布には必要な依存物を含める必要があるんだぜ。

つまり、

> **配布できるんだぜ。**

と、

> **配布が何も考えなくていいんだぜ。**

は別なんだぜ。

## OSごとにbuildすることもあるんだぜ

PyInstallerはmulti-platformなんだぜ。

でも **cross-compilerではない** んだぜ。

公式ドキュメントでも、

```text
Windows向けはWindowsでbuildするんだぜ
Linux向けはLinuxでbuildするんだぜ
macOS向けはmacOSでbuildするんだぜ
```

という前提なんだぜ。

だから、

> **Windowsで一回buildしたら、そのexeをLinuxでも動かすんだぜ。**

ではないんだぜ。

これはPythonだから特別変というより、

**OS向けアプリ配布では普通に出てくる問題なんだぜ。**

## 「ソースが見えるから配れない」も別の話なんだぜ

Pythonはsource codeが読みやすいんだぜ。

だから、

> **配布したら中身を見られるんだぜ。**

を気にする人もいるんだぜ。

PyInstallerも内部にcompiled Python codeを持つんだぜ。

なので、

> **onefileにしたから完全に秘密になるんだぜ。**

ではないんだぜ。

でもこれも、

```text
一般配布できるか
```

とは別の問題なんだぜ。

native binaryだってreverse engineeringはできるんだぜ。

難しさが違うだけなんだぜ。

**配布と秘匿は別問題なんだぜ。**

## 「スクリプトだから製品にはできない」も違うんだぜ

ここまで来ると分かるんだぜ。

ソフトウェア製品として大事なのは、

```text
どう起動するんだぜ
依存物をどう持っていくんだぜ
アップデートをどうするんだぜ
設定ファイルをどうするんだぜ
署名するんだぜ
installerを作るんだぜ
クラッシュ時どうするんだぜ
```

なんだぜ。

中身が、

```text
Cなんだぜ
C++なんだぜ
Rustなんだぜ
Pythonなんだぜ
Javaなんだぜ
C#なんだぜ
```

のどれかだけで、

> **一般配布できる / できない**

が決まるわけじゃないんだぜ。

## だからなんだぜ

Pythonは確かに、

```text
.pyをpythonで実行するんだぜ
```

という使い方が分かりやすい言語なんだぜ。

だから、

> **相手にもPythonを入れてもらわないとダメなんだぜ。**

と思いやすいんだぜ。

でも実際には、

```text
Pythonが必要なんだぜ
    ↓
じゃあPythonも一緒に配るんだぜ
```

でいいんだぜ。

PyInstallerみたいに処理系ごとbundleする方法もあるんだぜ。

Nuitkaみたいにstandalone / onefileへ持っていく方法もあるんだぜ。

Pythonがある環境ならzipappやpipで配る方法もあるんだぜ。

**Pythonはスクリプト言語だから一般配布できない、なんてことはないんだぜ。**

**必要な実行環境ごと、ちゃんと配ればいいんだぜ。**

### Sources

- PyInstaller Manual  
  https://pyinstaller.org/en/stable/
- PyInstaller, *Using PyInstaller*  
  https://pyinstaller.org/en/stable/usage.html
- PyInstaller, *Advanced Topics*  
  https://pyinstaller.org/en/latest/advanced-topics.html
- Nuitka, *Tutorial Setup and Build*  
  https://nuitka.net/user-documentation/tutorial-setup-and-build.html
- Nuitka, *User Manual*  
  https://nuitka.net/user-documentation/user-manual.html
- Python documentation, *zipapp — Manage executable Python zip archives*  
  https://docs.python.org/3/library/zipapp.html
