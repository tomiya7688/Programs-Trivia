# #020 AIソフトってな、重みファイルだけじゃないんだぜ

AIの話をしてると、

```text
このモデルは何GBなんだぜ
重みをダウンロードしたんだぜ
モデルを公開したんだぜ
```

みたいな話が出てくるんだぜ。

すると、

> **AIソフト = 重みファイルなんだぜ。**

みたいに見えてくるんだぜ。

でもな。

**重みはAIの中核ではあるけど、重みだけではAIソフトにならないんだぜ。**

これはLLMだけの話じゃないんだぜ。

画像認識でも、物体検出でも、音声認識でも、セグメンテーションでも同じなんだぜ。

## セグメンテーションを見ると分かりやすいんだぜ

例えば画像から道路だけを塗り分ける **semantic segmentation** モデルがあるんだぜ。

重みファイルは同じなんだぜ。

でも実際のAIソフトは、ざっくりこうなんだぜ。

```text
カメラ画像なんだぜ
    ↓
resizeするんだぜ
    ↓
色や値をnormalizeするんだぜ
    ↓
tensorにするんだぜ
    ↓
モデルで推論するんだぜ
    ↓
logitsが出るんだぜ
    ↓
argmax / thresholdするんだぜ
    ↓
元画像サイズへ戻すんだぜ
    ↓
小さいゴミを消すんだぜ
    ↓
穴を埋めるんだぜ
    ↓
必要なら輪郭や面積を計算するんだぜ
    ↓
製品としての答えを出すんだぜ
```

なんだぜ。

モデルがやってるのは、この真ん中の一部分なんだぜ。

Hugging Faceのimage processorだって、公式に **resize・normalization・tensor化・segmentation maskへのpost-process** まで担当するものとして説明されてるんだぜ。

つまり、

> **モデルを動かす前と後にも、AIの結果を決める処理が普通にあるんだぜ。**

なんだぜ。

## 同じ重みでも前処理を変えたら結果が変わるんだぜ

例えば学習時に、

```text
512 × 512なんだぜ
RGBなんだぜ
meanを引くんだぜ
stdで割るんだぜ
```

という入力を使っていたモデルがあるんだぜ。

推論時に、

```text
resize方法を変えるんだぜ
cropするんだぜ
BGRのまま入れるんだぜ
normalizationを忘れるんだぜ
```

としたら、同じ重みでも当然結果は変わるんだぜ。

極端な話、

> **モデルが悪いんだぜ。**

と思ってたら、

> **前処理が学習時と違ったんだぜ。**

なんてこともあるんだぜ。

**AIの性能は重みだけで決まってないんだぜ。**

## 後処理だけでも精度が変わるんだぜ

セグメンテーションモデルが、

```text
このpixelは道路 0.51なんだぜ
このpixelは道路 0.49なんだぜ
```

みたいな出力を返したとするんだぜ。

そこで、

```text
0.5以上なら道路なんだぜ
```

にするか、

```text
0.7以上なら道路なんだぜ
```

にするかでmaskは変わるんだぜ。

さらに、

```text
小さい領域を消すんだぜ
穴を埋めるんだぜ
morphologyするんだぜ
connected componentで選別するんだぜ
輪郭を滑らかにするんだぜ
```

を入れれば、最終的なIoUやprecision / recallまで変わり得るんだぜ。

つまり、

> **同じモデルなんだぜ。**

なのに、

> **製品として見える精度が違うんだぜ。**

が普通に起きるんだぜ。

## 「何を推論して、何を出すか」もAIソフトの一部なんだぜ

これがかなり大事なんだぜ。

モデルが出すものと、ユーザーが欲しいものは同じとは限らないんだぜ。

例えば工場の外観検査なら、モデルはpixelごとのsegmentationを出すんだぜ。

でも製品として欲しいのは、

```text
傷があるんだぜ？
傷は何個なんだぜ？
面積はいくつなんだぜ？
3mm以上だけ不良なんだぜ？
製品を排出するんだぜ？
```

かもしれないんだぜ。

つまり、

```text
モデル出力
    ↓
業務ルール
    ↓
最終判定
```

なんだぜ。

同じsegmentation重みを使っても、

```text
A社
    1pixelでもあればNGなんだぜ

B社
    100pixel以上ならNGなんだぜ

C社
    特定領域だけ見るんだぜ
```

なら、同じAIモデルでも **AIソフトとしての答えは違う** んだぜ。

**AIが何を答えるかは、重みだけじゃなく「何を答えにするか」の設計でも決まるんだぜ。**

## CPUかGPUかでも、同じモデルの速さは全然違うんだぜ

次は精度じゃなくて実行性能なんだぜ。

ONNX Runtimeは、

```text
CPUなんだぜ
CUDAなんだぜ
TensorRTなんだぜ
OpenVINOなんだぜ
DirectMLなんだぜ
CoreMLなんだぜ
QNNなんだぜ
```

みたいな **Execution Provider** を切り替えて、同じONNXモデルを違うハードウェア向けに動かせるんだぜ。

公式ドキュメントにも、

> **CPU、GPU、FPGA、NPUなどの計算能力を使って最適に実行するんだぜ。**

という設計が出てるんだぜ。

つまり、

> **重みが同じなら速度も同じなんだぜ。**

じゃないんだぜ。

```text
CPUで動かすんだぜ
GPUで動かすんだぜ
TensorRT engineにするんだぜ
FP16にするんだぜ
INT8にするんだぜ
batchを変えるんだぜ
memory copyを減らすんだぜ
```

でlatencyもthroughputも変わるんだぜ。

NVIDIA TensorRTなんて、そのための **deep learning inference optimizer / runtime** なんだぜ。

## GPUにすれば必ず速いわけでもないんだぜ

ここも面白いんだぜ。

ONNX Runtimeの公式troubleshootingにも、

> **GPUなのにCPUより遅いんだぜ。**

という項目が普通にあるんだぜ。

理由には、

```text
GPUが対応してないoperatorがあるんだぜ
途中でCPUへfallbackするんだぜ
CPU↔GPUの転送が多いんだぜ
小さすぎてGPUを使うコストの方が高いんだぜ
```

みたいなのがあるんだぜ。

つまりAIソフトの速度は、

**モデルのFLOPSだけ見ても分からないんだぜ。**

周辺の実行設計まで含めて初めて分かるんだぜ。

## 大きい画像なら「どう切るか」だけでも精度と速度が変わるんだぜ

例えば4K画像をsegmentationするんだぜ。

モデル入力は512×512なんだぜ。

じゃあどうするんだぜ。

```text
4Kを512へ縮小するんだぜ？
512ずつtileするんだぜ？
tileを重ねるんだぜ？
境界部分を捨てるんだぜ？
複数scaleで推論するんだぜ？
```

全部結果が違うんだぜ。

全体を縮小すれば速いんだぜ。

でも小さい傷が消えるかもしれないんだぜ。

tileすれば細かいものが見えるんだぜ。

でも推論回数が増えるんだぜ。

overlapすれば境界はきれいになるかもしれないんだぜ。

でもさらに遅くなるんだぜ。

**同じ重みでも、推論戦略だけで精度と速度のバランスが変わるんだぜ。**

## LLMでも同じ構図なんだぜ

LLMはこの構図が分かりやすく見えるだけなんだぜ。

```text
tokenizerなんだぜ
chat templateなんだぜ
samplingなんだぜ
system promptなんだぜ
RAGなんだぜ
tool useなんだぜ
safety filterなんだぜ
retryなんだぜ
```

が周りに付くんだぜ。

同じ重みでも、

```text
temperatureを変えるんだぜ
RAGを付けるんだぜ
検索を付けるんだぜ
toolを付けるんだぜ
```

だけで、かなり別のAI製品みたいに見えるんだぜ。

でもこれはLLM特有の話じゃないんだぜ。

**AIというのは昔から、モデルの前後に普通のプログラムが大量に付いて完成するものなんだぜ。**

## 重みは「脳」っぽいけど、それだけじゃ製品にならないんだぜ

ざっくり分けるなら、

```text
入力
    カメラなんだぜ
    マイクなんだぜ
    センサーなんだぜ
    テキストなんだぜ

前処理
    resizeなんだぜ
    normalizeなんだぜ
    cropなんだぜ
    tokenizeなんだぜ

モデル
    architectureなんだぜ
    weightsなんだぜ

実行系
    CPUなんだぜ
    GPUなんだぜ
    NPUなんだぜ
    runtimeなんだぜ
    quantizationなんだぜ

後処理
    thresholdなんだぜ
    argmaxなんだぜ
    NMSなんだぜ
    morphologyなんだぜ
    decodeなんだぜ

業務ロジック
    何を正解にするんだぜ
    どこからNGなんだぜ
    次に何をするんだぜ

アプリ
    UIなんだぜ
    APIなんだぜ
    loggingなんだぜ
    retryなんだぜ
    monitoringなんだぜ
```

なんだぜ。

この全部を合わせて、ようやくユーザーが触る **AIソフト** になるんだぜ。

## だから「同じモデルなのに性能が違う」は普通なんだぜ

例えば同じsegmentation modelを使っていても、

```text
製品A
    高解像度tileなんだぜ
    overlapありなんだぜ
    morphologyありなんだぜ
    TensorRT FP16なんだぜ

製品B
    全体を小さくresizeなんだぜ
    post-processなしなんだぜ
    CPUなんだぜ
```

なら、

```text
精度が違うんだぜ
速度が違うんだぜ
メモリ使用量が違うんだぜ
最終的な判定が違うんだぜ
```

のが当然なんだぜ。

重みだけ比較して、

> **同じモデルだから同じAIなんだぜ。**

とは言えないんだぜ。

## だからなんだぜ

AIを見て、

> **このAIの正体はこの重みファイルなんだぜ。**

と思うと、半分しか見えてないんだぜ。

重みは確かに中心なんだぜ。

でも本当に動いてるAIソフトは、

```text
入力の取り方なんだぜ
+
前処理なんだぜ
+
重みなんだぜ
+
推論ランタイムなんだぜ
+
CPU/GPU最適化なんだぜ
+
後処理なんだぜ
+
何を最終出力にするかなんだぜ
+
周辺スクリプトなんだぜ
```

なんだぜ。

**AIソフトってのは、重みのことじゃないんだぜ。**

**何を入れて、どう推論して、どう加工して、何を答えとして出すかまで全部ひっくるめてAIソフトなんだぜ。**

### Sources

- Hugging Face Transformers, *Image Processor*  
  https://huggingface.co/docs/transformers/en/main_classes/image_processor
- ONNX Runtime, *Execution Providers*  
  https://onnxruntime.ai/docs/execution-providers/
- ONNX Runtime, *Troubleshooting performance*  
  https://onnxruntime.ai/docs/performance/tune-performance/troubleshooting.html
- NVIDIA, *TensorRT Documentation*  
  https://docs.nvidia.com/deeplearning/tensorrt/latest/
- Hugging Face Transformers, *Writing a chat template*  
  https://huggingface.co/docs/transformers/chat_templating_writing
- ggml-org, *llama.cpp — LLM inference in C/C++*  
  https://github.com/ggml-org/llama.cpp
