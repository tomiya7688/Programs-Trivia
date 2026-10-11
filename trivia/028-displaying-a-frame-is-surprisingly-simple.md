# #028 モニターに映すってな、メインコンピュータから見ると案外単純なんだぜ

画面に、

```text
ゲームなんだぜ
動画なんだぜ
ブラウザなんだぜ
3Dなんだぜ
ウィンドウなんだぜ
```

が映ってるんだぜ。

ものすごく複雑そうなんだぜ。

実際、

> **その絵を作る**

のはかなり複雑なんだぜ。

でもな。

**できあがった一枚の絵をモニターへ映す、という部分だけ見ると案外単純なんだぜ。**

ざっくり言えば、

> **画面用のメモリに絵を置いて、それを表示回路に上から順番に読ませるんだぜ。**

なんだぜ。

## 画面はまずメモリの中にあるんだぜ

ディスプレイへ出す画像は、まず **framebuffer** みたいなメモリ領域として存在するんだぜ。

例えば1pixelが、

```text
Rなんだぜ
Gなんだぜ
Bなんだぜ
Aなんだぜ
```

の4byteだとするんだぜ。

メモリには、

```text
pixel 0なんだぜ
pixel 1なんだぜ
pixel 2なんだぜ
...
```

と画面の色が並んでるんだぜ。

Linuxの古いframebuffer APIなんてかなり露骨で、

> **framebuffer deviceは普通のmemory deviceみたいにread/writeできるんだぜ。**

という設計なんだぜ。

pixel formatに合わせてメモリを書けば、それが表示用の画像になるんだぜ。

## 昔の感覚なら「このメモリに色を書くだけ」なんだぜ

ものすごく単純化すると、

```c
framebuffer[y * width + x] = red;
```

なんだぜ。

これで、

> **x, yのpixelを赤にしたんだぜ。**

という世界なんだぜ。

もちろん実際には、

```text
pixel formatなんだぜ
strideなんだぜ
memory mappingなんだぜ
cacheなんだぜ
double bufferingなんだぜ
```

とかがあるんだぜ。

でも考え方は、

**画面を表すメモリへ数字を書く**

なんだぜ。

Linux kernelのframebuffer documentationでも、pixelはmemoryにhardware-dependent formatで保存されて、applicationはそのformatに合わせてimage dataを書く仕組みになってるんだぜ。

## 現代GPUでも最後はframebufferへ行くんだぜ

現代のデスクトップはもっと複雑なんだぜ。

例えば3Dゲームなら、

```text
CPUがcommandを作るんだぜ
    ↓
GPUがvertexを計算するんだぜ
    ↓
rasterizeするんだぜ
    ↓
shaderを動かすんだぜ
    ↓
textureを読むんだぜ
    ↓
最終画像をrender targetへ書くんだぜ
```

なんだぜ。

ブラウザやdesktopなら、

```text
各windowを描くんだぜ
    ↓
compositorが重ねるんだぜ
    ↓
最終画面を作るんだぜ
```

なんだぜ。

ここまでは大変なんだぜ。

でも最後には、

> **今回モニターへ出す完成画像はこれなんだぜ。**

というbufferができるんだぜ。

そこから先はdisplay controllerの仕事なんだぜ。

## display controllerは上から順に読むんだぜ

LinuxのKMS documentationでは、この処理を **scanout** と呼んでるんだぜ。

ざっくり、

```text
framebufferなんだぜ
    ↓
display engineが読むんだぜ
    ↓
1行目なんだぜ
2行目なんだぜ
3行目なんだぜ
...
    ↓
モニターへ送るんだぜ
```

なんだぜ。

kernel documentationにも、

> **display imageをtopからbottomへ、1 row of pixelsずつscan outするんだぜ。**

という説明があるんだぜ。

AMDのdisplay documentationでも、

```text
memoryからframe informationを読むんだぜ
必要な変換をするんだぜ
pixel dataをdisplayへ送るんだぜ
```

がdisplay pipeの役割として書かれてるんだぜ。

つまり、

**絵が完成したあとの表示処理は、かなり「メモリを順番に読む」に近いんだぜ。**

## CPUが毎pixelをHDMIへ送ってるわけじゃないんだぜ

ここが重要なんだぜ。

メインCPUが、

```text
pixel 1送るんだぜ
pixel 2送るんだぜ
pixel 3送るんだぜ
...
```

と60fpsずっと手作業してるわけじゃないんだぜ。

一度、

> **このbufferを、このmodeで表示するんだぜ。**

とdisplay hardwareへ設定するんだぜ。

するとdisplay controllerが自動で、

```text
memoryから読むんだぜ
timingを作るんだぜ
scanoutするんだぜ
connectorへ流すんだぜ
```

を繰り返すんだぜ。

メインCPUから見れば、

> **表示するbufferを指定したんだぜ。あとはdisplay回路よろしくなんだぜ。**

にかなり近いんだぜ。

## Linux KMSの構造がそのまんまなんだぜ

LinuxのDRM/KMSでは、表示pipelineをざっくり、

```text
framebufferなんだぜ
    ↓
planeなんだぜ
    ↓
CRTCなんだぜ
    ↓
encoder / connectorなんだぜ
    ↓
monitorなんだぜ
```

みたいに扱うんだぜ。

framebufferはpixelのsourceなんだぜ。

planeは、

```text
どのbufferを使うんだぜ
どこに置くんだぜ
拡大縮小するんだぜ
回転するんだぜ
重ね順は何番なんだぜ
```

を決めるんだぜ。

CRTCは複数planeをblendして、表示timingに合わせてscanoutするんだぜ。

つまりOSから見ても、

> **この画像をこの場所に出すんだぜ。**

という設定をdisplay pipelineへ渡す設計なんだぜ。

## windowを動かしても、全部CPUで描き直してるとは限らないんだぜ

今のdisplay controllerには複数の **plane** があることがあるんだぜ。

例えば、

```text
desktopなんだぜ
videoなんだぜ
cursorなんだぜ
```

を別々のbufferとして渡して、display hardware側で重ねられることがあるんだぜ。

cursorなんかは典型なんだぜ。

マウスを少し動かすたびに、

> **画面全部をCPUで描き直すんだぜ。**

じゃなくて、

> **cursor planeの位置だけ変えるんだぜ。**

みたいなことができるんだぜ。

ここでもメインコンピュータは、

**細かい表示作業を専用回路へ丸投げしてるんだぜ。**

## double bufferingも「次の絵に切り替える」だけなんだぜ

画面を書いてる途中でdisplay controllerが同じbufferを読んだら、

```text
上半分は新しいframeなんだぜ
下半分は古いframeなんだぜ
```

みたいな **tearing** が起きることがあるんだぜ。

そこで、

```text
buffer A
    今表示してるんだぜ

buffer B
    次のframeを描いてるんだぜ
```

にするんだぜ。

Bが完成したら、

> **次からBを表示するんだぜ。**

と切り替えるんだぜ。

これがpage flipの基本的な感覚なんだぜ。

つまり、

**完成した絵を別のbufferへ作って、表示先を切り替える**

なんだぜ。

## でもHDMIやDisplayPort自体はちゃんと複雑なんだぜ

ここで、

> **じゃあモニター出力なんて簡単なんだぜ。**

まで言うと違うんだぜ。

HDMIやDisplayPortの物理層では、

```text
pixel clockなんだぜ
link trainingなんだぜ
encodingなんだぜ
laneなんだぜ
blankingなんだぜ
audioなんだぜ
color formatなんだぜ
HDCPなんだぜ
EDIDなんだぜ
HDR metadataなんだぜ
```

みたいな話があるんだぜ。

高解像度・高refresh rateになれば信号品質も大変なんだぜ。

でも大事なのは、

> **その複雑さを毎回applicationがやるわけじゃないんだぜ。**

ということなんだぜ。

driverとdisplay controllerとPHYが吸収してくれるんだぜ。

**上の層からは単純に見えるように、下の層が頑張ってるんだぜ。**

## 「描く」と「映す」は別なんだぜ

ここを分けると分かりやすいんだぜ。

### 描く

```text
3D計算するんだぜ
fontをrasterizeするんだぜ
windowを合成するんだぜ
動画をdecodeするんだぜ
shaderを動かすんだぜ
```

なんだぜ。

かなり複雑なんだぜ。

### 映す

```text
完成したbufferを指定するんだぜ
    ↓
display controllerが順番に読むんだぜ
    ↓
monitorへ送るんだぜ
```

なんだぜ。

こっちは概念としてはかなり素朴なんだぜ。

## #008や#026と同じ構造なんだぜ

前に、

> **昔の周辺機器は専用回路で処理してたんだぜ。**

とか、

> **今は小さいコンピュータが周辺機器に入ってるんだぜ。**

って話をしたんだぜ。

モニター出力も似てるんだぜ。

メインCPUが何でも一個ずつ面倒を見るんじゃないんだぜ。

```text
GPUなんだぜ
display controllerなんだぜ
encoderなんだぜ
PHYなんだぜ
monitor controllerなんだぜ
```

へ仕事を分けてるんだぜ。

だからメイン側から見ると、

**ものすごく複雑な画面出力が、かなり単純なinterfaceに見えるんだぜ。**

## だからなんだぜ

画面に4Kのゲームが映ってるんだぜ。

HDRなんだぜ。

144Hzなんだぜ。

ものすごいことをしてるように見えるんだぜ。

実際、絵を作る方はものすごいんだぜ。

でも表示だけを突き詰めると、

> **このメモリが次の画面なんだぜ。**

と指定して、

> **display controller、上から順番に読んでモニターへ送り続けるんだぜ。**

なんだぜ。

**モニターに映すという行為は、メインコンピュータから見ると案外単純なんだぜ。**

**複雑なのは、その単純な操作に見えるように下の専用回路が全部隠してくれてるからなんだぜ。**

### Sources

- Linux Kernel Documentation, *The Frame Buffer Device*  
  https://docs.kernel.org/fb/framebuffer.html
- Linux Kernel Documentation, *The Frame Buffer Device API*  
  https://docs.kernel.org/fb/api.html
- Linux Kernel Documentation, *Kernel Mode Setting (KMS)*  
  https://docs.kernel.org/gpu/drm-kms.html
- Linux Kernel Documentation, *AMD Display Core*  
  https://docs.kernel.org/gpu/amdgpu/display/index.html
