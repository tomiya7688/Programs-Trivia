# Programs-Trivia

プログラムの「実はこうなんだぜ」を集めるリポジトリなんだぜ。

コードそのものを書くというより、言語の仕様、歴史、変な挙動、よくある誤解なんかを「実はこうなんだぜ」のノリで眺めていくんだぜ。

---

## #001 クラスはオブジェクト指向より前なんだぜ

今だと、

**クラス → オブジェクト → オブジェクト指向**

みたいな順番で習うことが多いんだぜ。

だから「オブジェクト指向という考え方が先にあって、そのためにクラスが作られた」と思いがちなんだぜ。

でも歴史はそんなに素直じゃないんだぜ。

### 1965年にはもう `record class` がいたんだぜ

1965年、C. A. R. Hoare は ALGOL の拡張案として **Record Handling** を発表しているんだぜ。

そこではすでに、

```
record class
```

という言葉が使われていて、レコードをクラスに所属させたり、サブクラスを扱ったりする考え方まで出てくるんだぜ。

つまり、現在のクラスにつながる系譜は「オブジェクト指向」という名前が確立するより前から育っていたんだぜ。

### それを Simula が拾ったんだぜ

その後、Ole-Johan Dahl と Kristen Nygaard が Simula 67 を作る過程で、Hoare の record class / subclass の考え方を取り込みながら、**class と、その class に属する object** という形へ発展させていったんだぜ。

Simula 67 ではクラスが単なるデータの型ではなく、処理も持てるようになって、継承につながる仕組みも入ってきたんだぜ。

ここまで来ると、かなり今の「クラス」の顔になってくるんだぜ。

### 「オブジェクト指向」という名前は後から来たんだぜ

Alan Kay と Smalltalk の流れで **object-oriented programming** という言葉が生まれ、オブジェクトを中心にプログラムを考える思想がはっきり名前を持つようになったんだぜ。

だから、

> **オブジェクト指向が完成して、その部品としてクラスが発明されたわけじゃないんだぜ。**
>
> **クラスやオブジェクトにつながる仕組みが先に育って、その流れが後から「オブジェクト指向」という名前を持ったんだぜ。**

という見方ができるんだぜ。

### ただし厳密には、なんだぜ

Simula 67 は一般に「最初のオブジェクト指向言語」とされているんだぜ。

だから「クラスはオブジェクト指向より前なんだぜ」というのは、

**クラスの系譜や用語が、オブジェクト指向という名前・確立したパラダイムより先に現れていたんだぜ**

という意味なんだぜ。

こういう「教科書で見る順番と、歴史上できた順番は違うんだぜ」が今回の雑学なんだぜ。

---

### Sources

- C. A. R. Hoare, *Record Handling*, ALGOL Bulletin No. 21, 1965  
  https://softwarepreservation.computerhistory.org/ALGOL/standards.html
- Computer History Museum, *Papers on the history of ALGOL*  
  https://softwarepreservation.computerhistory.org/ALGOL/history.html
- Computer History Museum, *Introducing the Smalltalk Zoo*  
  https://computerhistory.org/blog/introducing-the-smalltalk-zoo-48-years-of-smalltalk-history-at-chm/
