# #030 リレー式コンピュータだって、ちゃんとプログラミングできるんだぜ

リレー式コンピュータって聞くと、

```text
カチカチ動くんだぜ
配線で計算するんだぜ
決まった計算しかできないんだぜ
```

みたいなイメージになりやすいんだぜ。

でもな。

**リレーでできたコンピュータでも、命令列を外から与えて処理を変えられるなら、ちゃんとプログラミングできるんだぜ。**

## Z3はリレーなのに「プログラム制御」だったんだぜ

1941年、Konrad Zuseが作った **Z3** があるんだぜ。

こいつは約2,300個のリレーを使ったelectromechanical computerなんだぜ。

22bitの浮動小数点演算までやるんだぜ。

そして重要なのが、

> **program-controlledなんだぜ。**

なんだぜ。

Computer History Museumも、Z3を **世界初のprogram-controlled electromechanical digital computer** と説明してるんだぜ。

## プログラムは映画フィルムに穴を開けて入れたんだぜ

ここが最高なんだぜ。

Z3のプログラムは、

```text
SSDなんだぜ
RAMなんだぜ
USBメモリなんだぜ
```

に入ってたわけじゃないんだぜ。

**穴を開けたフィルム** なんだぜ。

しかもZuseは使い古しの映画フィルムまで利用してたんだぜ。

```text
命令を決めるんだぜ
    ↓
filmに穴を開けるんだぜ
    ↓
機械に読ませるんだぜ
    ↓
relayがカチカチ動くんだぜ
    ↓
計算するんだぜ
```

なんだぜ。

つまり、

> **プログラムを書く**

が文字どおり、

> **フィルムへ穴を開ける**

だったんだぜ。

## リレーはCPUの「部品」なんだぜ

前に、

> **0と1ってのは、突き詰めるとスイッチみたいな二つの状態なんだぜ。**

って話をしたんだぜ。

リレーはまさに物理的なスイッチなんだぜ。

電磁石を使って、

```text
接点を閉じるんだぜ
接点を開くんだぜ
```

をやるんだぜ。

だからリレーを組み合わせれば、

```text
ANDなんだぜ
ORなんだぜ
NOTなんだぜ
記憶なんだぜ
加算なんだぜ
制御なんだぜ
```

を作れるんだぜ。

半導体のtransistorでやってる論理を、

**もっと大きくて遅い電磁スイッチでやるんだぜ。**

なんだぜ。

だから、

> **リレーだからプログラムできないんだぜ。**

では全然ないんだぜ。

## Harvard Mark Iも命令をpaper tapeから読んだんだぜ

アメリカでは **Harvard Mark I** があるんだぜ。

1944年に完成した巨大なelectromechanical machineなんだぜ。

Computer History Museumによると、

```text
約51 feetなんだぜ
約5 tonsなんだぜ
3,500 relaysなんだぜ
```

という怪物なんだぜ。

こいつも、

> **instructionsをpaper tapeから読むんだぜ。**

なんだぜ。

つまり、

```text
命令1なんだぜ
命令2なんだぜ
命令3なんだぜ
...
```

というsequenceを紙テープへパンチして、機械に食わせるんだぜ。

これも立派なプログラムなんだぜ。

## 「プログラム」はRAMに入ってなくてもいいんだぜ

今の感覚だと、

```text
プログラム
    ↓
SSDから読むんだぜ
    ↓
RAMへloadするんだぜ
    ↓
CPUがfetchするんだぜ
```

なんだぜ。

だから、

> **programはmemoryに保存されるものなんだぜ。**

と思いがちなんだぜ。

でもこれは **stored-program computer** が一般化した後の感覚なんだぜ。

Z3やHarvard Mark Iみたいな初期の機械では、

```text
data memoryなんだぜ
programなんだぜ
```

が別なんだぜ。

プログラムは、

```text
film tapeなんだぜ
paper tapeなんだぜ
punched cardなんだぜ
plugboardなんだぜ
```

みたいな外部媒体から与えられることがあるんだぜ。

**プログラムとは「命令を変更可能な形で与えること」であって、RAMに入ってることではないんだぜ。**

## しかも昔の「プログラミング」は制約だらけなんだぜ

もちろん今のPythonみたいにはいかないんだぜ。

```python
for i in range(100):
    print(i)
```

なんて書けないんだぜ。

Z3の命令セットはかなり小さいんだぜ。

もともとのZ3には現代的なconditional branchもないんだぜ。

Harvard Mark Iも初期状態では、今のCPUみたいな自由な分岐を持ってなかったんだぜ。

だから、

> **プログラム可能なんだぜ。**

と、

> **現代の汎用CPUみたいに何でも自由に書けるんだぜ。**

は別なんだぜ。

でも、

```text
どの演算をするんだぜ
どの順番でやるんだぜ
どのdataを使うんだぜ
```

を外から変えられるんだぜ。

それだけでも、

**固定機能の計算機とは決定的に違うんだぜ。**

## 「配線を変える」だけでもプログラミングになり得るんだぜ

さらに昔の機械では、

```text
plugboardを差し替えるんだぜ
switchを設定するんだぜ
配線を変えるんだぜ
```

ことで処理を変更するものもあるんだぜ。

今の感覚だと、

> **それhardware改造じゃないんだぜ？**

って思うんだぜ。

でも当時は、

> **機械にどの処理をさせるか設定する**

こと自体がプログラミングなんだぜ。

ソースコードを書くことだけがプログラミングじゃないんだぜ。

## だから「programming language」より先にprogrammingがあるんだぜ

今は、

```text
Pythonなんだぜ
Cなんだぜ
Rustなんだぜ
JavaScriptなんだぜ
```

を書いてprogrammingするんだぜ。

でも昔は、

```text
穴の位置なんだぜ
switchの位置なんだぜ
plugboardなんだぜ
machine instructionなんだぜ
```

でprogrammingしてたんだぜ。

つまり、

> **人間が読みやすいprogramming languageが先にあってcomputerができたんだぜ。**

ではないんだぜ。

**機械へ変更可能な命令を与える行為が先にあって、その上にprogramming languageが育ったんだぜ。**

## Zuseはそのあと高級言語まで考えたんだぜ

しかもKonrad Zuseは、リレー式computerを作るだけで終わらなかったんだぜ。

その後 **Plankalkül** という初期のhigh-level programming languageまで考えたんだぜ。

Computer History Museumも、Zuseを、

```text
program-controlled electromechanical digital computerを発明した人なんだぜ
+
最初期のhigh-level programming languageを考えた人なんだぜ
```

として評価してるんだぜ。

つまり、

> **relayをカチカチ動かしてた人が、その先で高級言語まで考えてたんだぜ。**

なんだぜ。

歴史が一気につながるんだぜ。

## 電子式になったからプログラミングできるようになったわけじゃないんだぜ

これがこの話の一番大事なところなんだぜ。

```text
relay
    遅いんだぜ

vacuum tube
    速いんだぜ

transistor
    もっと小さくて速いんだぜ
```

という技術の進歩はあるんだぜ。

でも、

> **programmingできるかどうか**

は、

> **switchがrelayかtransistorか**

では決まらないんだぜ。

**命令を外から変更可能な形で与えて、機械がその順番に従って処理できるか**

なんだぜ。

だから、

> **relay computerは古いcalculatorなんだぜ。**

だけではないんだぜ。

ちゃんと、

**programを読み、命令に従って動くcomputerが存在したんだぜ。**

## だからなんだぜ

1941年なんだぜ。

半導体CPUなんてないんだぜ。

RAMもSSDもないんだぜ。

リレーがカチカチ鳴ってるんだぜ。

でも、

```text
filmへ命令をパンチするんだぜ
    ↓
computerへ読ませるんだぜ
    ↓
命令順を変えるんだぜ
    ↓
別の計算をさせるんだぜ
```

なんだぜ。

**リレー式コンピュータだって、ちゃんとプログラミングできるんだぜ。**

**プログラムってのは、ソースコードファイルのことじゃないんだぜ。機械へ「次に何をするんだぜ」を変更可能な形で与えることなんだぜ。**

### Sources

- Computer History Museum, *Konrad Zuse*  
  https://computerhistory.org/profile/konrad-zuse/
- Computer History Museum, *1941 — Konrad Zuse finishes the Z3 Computer*  
  https://www.computerhistory.org/timeline/1941/
- Deutsches Museum, *Computers — the History of the Calculating Machine*  
  https://www.deutsches-museum.de/en/museumsinsel/ausstellung/computers
- Computer History Museum, *Harvard Mark I is completed*  
  https://www.computerhistory.org/timeline/1944/
- Computer History Museum, *Harvard and IBM Agree to Build The Mark I*  
  https://www.computerhistory.org/tdih/march/31/
