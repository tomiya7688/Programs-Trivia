# #026 意外と何にでもコンピュータは入ってて、その中身は意外と単純なんだぜ

「コンピュータ」って聞くと、

```text
パソコンなんだぜ
スマホなんだぜ
サーバーなんだぜ
スーパーコンピュータなんだぜ
```

みたいな、画面があってOSが動いてるものを想像しがちなんだぜ。

でもな。

**実際には、もっと小さいコンピュータが身の回りの機械に大量に入ってるんだぜ。**

しかも、その中でやってることは意外と地味なんだぜ。

## キーボードの中にもいるんだぜ

キーボードなんて、

> **キーを押したら文字を送るだけなんだぜ。**

に見えるんだぜ。

でも中では小さいcontrollerが、

```text
列を1本ずつONにするんだぜ
行を読むんだぜ
どのキーが押されたか調べるんだぜ
チャタリングを無視するんだぜ
USB HIDのreportを作るんだぜ
PCへ送るんだぜ
```

みたいなことをずっとやってるんだぜ。

QMKの説明でも、microcontrollerが列を順番に駆動して行を読む **matrix scanning** でキーを検出する仕組みがそのまま出てるんだぜ。

つまりキーボードの中のコンピュータは、

> **行と列をひたすら巡回してるんだぜ。**

なんだぜ。

意外と地味なんだぜ。

## 洗濯機の中にもいるんだぜ

洗濯機はさらに分かりやすいんだぜ。

中のmicrocontrollerは、

```text
水位を見るんだぜ
温度を見るんだぜ
ドアが閉まってるか見るんだぜ
モーターの回転を見るんだぜ
給水弁を開けるんだぜ
ポンプを回すんだぜ
モーターを回すんだぜ
止めるんだぜ
```

みたいなことをやるんだぜ。

STの洗濯機向け資料でも、microcontrollerが洗濯cycle、man-machine interface、motor speed controlを担当する例が出てるんだぜ。

現代のreference designでも、STM32がモーター制御だけじゃなく、

```text
currentなんだぜ
voltageなんだぜ
temperatureなんだぜ
communicationなんだぜ
overcurrent protectionなんだぜ
overvoltage protectionなんだぜ
overtemperature protectionなんだぜ
```

まで面倒を見る構成があるんだぜ。

でもプログラムの骨格だけ見ると、

```text
センサー読むんだぜ
    ↓
今どの状態か見るんだぜ
    ↓
出力を変えるんだぜ
    ↓
またセンサー読むんだぜ
```

なんだぜ。

## 結局、状態機械なんだぜ

こういう組み込みソフトでは **state machine** がものすごく多いんだぜ。

例えば洗濯機なら、

```text
STATE_IDLEなんだぜ
    ↓
STATE_FILLなんだぜ
    ↓
STATE_WASHなんだぜ
    ↓
STATE_DRAINなんだぜ
    ↓
STATE_SPINなんだぜ
    ↓
STATE_DONEなんだぜ
```

みたいな感じなんだぜ。

そしてmain loopでは、

```c
while (1) {
    read_sensors();
    update_state();
    control_outputs();
}
```

みたいな構造になることがあるんだぜ。

もちろん実物はinterruptやRTOS、PWM、通信、故障診断なんかが入ってもっと複雑なんだぜ。

でも根っこは、

> **読む → 判断する → 動かす**

なんだぜ。

## USB-C充電器だって意外と賢いんだぜ

USB-Cの充電器も、

> **電気を出してるだけなんだぜ。**

じゃないんだぜ。

USB Power Deliveryでは、

```text
相手がつながったんだぜ
向きはどっちなんだぜ
何V欲しいんだぜ
何Aまで出していいんだぜ
このcableで大丈夫なんだぜ
過電圧じゃないんだぜ
```

みたいな交渉があるんだぜ。

TIのUSB-PD controllerには、power roleやdata role、battery charger integration、GPIOまで設定できるものがあるんだぜ。

一方で最近は、外付けMCUなしでこういう処理をやる専用controllerもあるんだぜ。

つまりここでは、

> **汎用microcontrollerを入れるんだぜ。**

の場合もあれば、

> **必要な状態機械とfirmwareを専用chipの中へ押し込むんだぜ。**

の場合もあるんだぜ。

でもやってることはやっぱり、

```text
接続を検出するんだぜ
条件を読むんだぜ
交渉するんだぜ
電源経路を切り替えるんだぜ
異常なら止めるんだぜ
```

なんだぜ。

## SSDの中にもちゃんとコンピュータがいるんだぜ

SSDなんて、

> **メモリの塊なんだぜ。**

に見えるんだぜ。

でもNAND Flashは、HDDみたいに好きな場所を何度でもそのまま上書きできないんだぜ。

だからSSD controllerとfirmwareが、

```text
どこへ書くんだぜ
どのblockを消すんだぜ
古いdataをどこへ移すんだぜ
書き込み回数をどう散らすんだぜ
errorをどう直すんだぜ
```

を管理するんだぜ。

Kingstonの説明でも、SSD firmwareは、

```text
hostとの通信なんだぜ
data storage / retrievalなんだぜ
wear levelingなんだぜ
error correctionなんだぜ
```

を管理するものとして説明されてるんだぜ。

garbage collectionもcontrollerがbackgroundでやるんだぜ。

つまりSSDの中では、

> **ユーザーがファイルを保存してない時間にも、小さいコンピュータが勝手に掃除してるんだぜ。**

なんだぜ。

## 実は「見えないコンピュータ」の方が多いんだぜ

身の回りを見渡すと、

```text
キーボードなんだぜ
マウスなんだぜ
洗濯機なんだぜ
エアコンなんだぜ
電子レンジなんだぜ
プリンタなんだぜ
カメラなんだぜ
充電器なんだぜ
SSDなんだぜ
車載ECUなんだぜ
電動工具なんだぜ
バッテリー管理なんだぜ
```

みたいなところに、

```text
microcontrollerなんだぜ
controllerなんだぜ
DSPなんだぜ
専用processorなんだぜ
```

が入ってるんだぜ。

でも普通は画面もshellもないんだぜ。

だから、

> **コンピュータが入ってるんだぜ。**

って意識しないんだぜ。

## しかもCPUはびっくりするほど小さくていいんだぜ

PCみたいに、

```text
8coreなんだぜ
16GB RAMなんだぜ
SSD 1TBなんだぜ
```

なんて必要ないんだぜ。

組み込みなら、

```text
数十MHzなんだぜ
RAMは数KB〜数百KBなんだぜ
Flashも数十KB〜数MBなんだぜ
```

くらいでも十分な仕事が山ほどあるんだぜ。

だって、

> **水位を読んで弁を閉じるんだぜ。**

なら、巨大な計算機はいらないんだぜ。

## 「単純」だから価値が低いわけじゃないんだぜ

ここは重要なんだぜ。

中身が単純でも、

> **簡単な仕事なんだぜ。**

とは限らないんだぜ。

洗濯機なら、

```text
モーターを壊さないんだぜ
人を感電させないんだぜ
異常時に安全停止するんだぜ
ノイズの中でも誤動作しないんだぜ
10年間動くんだぜ
```

が必要なんだぜ。

USB充電器なら、

```text
間違った電圧を出さないんだぜ
overcurrentで止まるんだぜ
cable条件を守るんだぜ
```

が必要なんだぜ。

SSDなら、

```text
電源が落ちても壊れにくいんだぜ
Flashの寿命を均等に使うんだぜ
bit errorを直すんだぜ
```

が必要なんだぜ。

だから、

**アルゴリズムが単純なのと、製品として簡単なのは別なんだぜ。**

## #008の昔の周辺機器とつながるんだぜ

前に、

> **小型コンピュータが安くなかった時代の周辺機器は、専用回路で処理してたんだぜ。**

って話をしたんだぜ。

今は逆なんだぜ。

昔なら、

```text
リレーなんだぜ
ダイオードなんだぜ
カウンタなんだぜ
専用ロジックなんだぜ
```

で組んでいた処理を、

```text
安いmicrocontrollerを1個入れるんだぜ
firmwareを書くんだぜ
```

で済ませられるんだぜ。

つまり、

> **小型コンピュータが安くなったから、何にでもコンピュータを入れるようになったんだぜ。**

なんだぜ。

## だからなんだぜ

「コンピュータ」って聞くと、

> **複雑な計算をする機械なんだぜ。**

と思いがちなんだぜ。

でも身の回りのコンピュータの多くは、

```text
入力を見るんだぜ
状態を覚えるんだぜ
条件を判断するんだぜ
出力を変えるんだぜ
また繰り返すんだぜ
```

をひたすらやってるんだぜ。

キーボードはキーを読むんだぜ。

洗濯機はセンサーを読むんだぜ。

充電器は電圧を交渉するんだぜ。

SSDはFlashを掃除するんだぜ。

**意外と何にでもコンピュータは入ってるんだぜ。**

**そしてその中身は、意外と「読む・判断する・動かす」の繰り返しなんだぜ。**

### Sources

- QMK Firmware, *How a Keyboard Matrix Works*  
  https://docs.qmk.fm/how_a_matrix_works
- STMicroelectronics, *ST62 microcontrollers drive home appliance motor technology*  
  https://www.st.com/resource/en/application_note/an885-st62-microcontrollers-drive-home-appliance-motor-technology-stmicroelectronics.pdf
- STMicroelectronics, *Washing machines*  
  https://www.st.com/en/applications/home-and-professional-appliances/washing-machines.html
- Texas Instruments, *TPS25751A USB Type-C and USB PD Controller*  
  https://www.ti.com/product/TPS25751A
- Kingston, *SSD FAQ*  
  https://www.kingston.com/en/ssd/ssd-faq
- Kingston, *What is SSD firmware?*  
  https://www.kingston.com/en/support/technical/products/sedc400s37
