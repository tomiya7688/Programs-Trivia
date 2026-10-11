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

### 重みって何なんだぜ

ニューラルネットワークには大量の数値が入ってるんだぜ。

```text
0.0312
-0.8821
1.2044
...
```

みたいな数字なんだぜ。

学習によって調整された、この大量の数値が **weights、重み** なんだぜ。

ざっくり言えば、

> **学習で身につけたパラメータなんだぜ。**

なんだぜ。

LLMなら数十億、数百億、それ以上のパラメータを持つこともあるんだぜ。

だからファイルサイズも巨大になるんだぜ。

でも、この数字の山だけ渡されても、

> **じゃあ会話してくれなんだぜ。**

とはならないんだぜ。

### 重みを実行するソフトが要るんだぜ

まず必要なのが **推論ランタイム** なんだぜ。

例えば、

```text
Transformersなんだぜ
llama.cppなんだぜ
vLLMなんだぜ
ONNX Runtimeなんだぜ
TensorRTなんだぜ
```

みたいなやつなんだぜ。

こいつらが、

```text
重みを読むんだぜ
GPUやCPUへ載せるんだぜ
行列演算するんだぜ
次のtokenを計算するんだぜ
```

をやるんだぜ。

`llama.cpp` だって公式READMEからして、

> **LLM inference in C/C++**

なんだぜ。

つまり重みファイルとは別に、

**重みを動かすプログラム**

がちゃんと存在するんだぜ。

### tokenizerがないと文章を入れられないんだぜ

LLMは文章をそのまま読んでるわけじゃないんだぜ。

```text
こんにちは
```

を、

```text
token 1281
token 982
token 44
```

みたいなtoken列に変換してからモデルへ入れるんだぜ。

これをやるのが **tokenizer** なんだぜ。

そして出てきたtokenを文章へ戻すのもtokenizerなんだぜ。

だから、

```text
入力文章
    ↓
tokenizer
    ↓
token列
    ↓
重みを使って推論
    ↓
token列
    ↓
detokenizer
    ↓
文章
```

なんだぜ。

重みだけあってtokenizerが違ったら、モデルが想定していた入力にならないんだぜ。

**AIの口と耳みたいなもんなんだぜ。**

### chat templateが違うだけで挙動が変わるんだぜ

これが結構重要なんだぜ。

チャットAIに、

```text
system
user
assistant
```

という役割があるように見えるんだぜ。

でもモデル内部に、

> **これはsystem messageなんだぜ。**

というJSON parserが入ってるわけじゃないんだぜ。

実際にはchat templateで、

```text
<|system|>
あなたは親切なAIなんだぜ
<|user|>
こんにちは
<|assistant|>
```

みたいな、モデルが学習時に見ていた文字列形式へ変換するんだぜ。

Hugging FaceのTransformersでもchat templateはJinjaテンプレートとして管理されてるんだぜ。

しかも公式ドキュメントには、

> **学習時と違うchat formatを使うと性能が落ちることがあるんだぜ。**

という趣旨まで書かれてるんだぜ。

つまり、

**重みが同じでも、周辺のテンプレートが違うだけで答え方が変わるんだぜ。**

### 「次のtokenをどう選ぶか」も重みの外なんだぜ

モデルは、

```text
次はA 40%
次はB 30%
次はC 10%
...
```

みたいな確率分布を出すんだぜ。

そこから実際にどのtokenを選ぶかは別の処理なんだぜ。

例えば、

```text
temperatureなんだぜ
top-kなんだぜ
top-pなんだぜ
repetition penaltyなんだぜ
seedなんだぜ
```

みたいな設定があるんだぜ。

同じ重みでも、

```text
temperature = 0
```

ならかなり固定的な答えになるんだぜ。

```text
temperature = 1.2
```

ならかなり散らばるかもしれないんだぜ。

**性格が変わったように見えても、重みは同じなんだぜ。**

### system promptも周辺ソフトなんだぜ

AIサービスには、

```text
あなたは○○として振る舞うんだぜ
この形式で返すんだぜ
この情報は出さないんだぜ
このツールを使うんだぜ
```

みたいなsystem promptが入ってることがあるんだぜ。

これも普通は重みの中身じゃないんだぜ。

サービス側が毎回入力へ付け足してるんだぜ。

だから同じモデルでも、

```text
プログラミング助手なんだぜ
旅行助手なんだぜ
社内FAQなんだぜ
法律文書の分類器なんだぜ
```

みたいに全然違う製品を作れるんだぜ。

### RAGは「外部の記憶」なんだぜ

企業向けAIなんかでは **RAG** がよく使われるんだぜ。

```text
質問されるんだぜ
    ↓
社内文書を検索するんだぜ
    ↓
関係ありそうな文章を取ってくるんだぜ
    ↓
それをpromptへ足すんだぜ
    ↓
モデルに答えさせるんだぜ
```

なんだぜ。

このとき、

```text
検索エンジン
embedding
vector database
reranker
文書分割
検索query生成
```

なんかは全部、重みの外なんだぜ。

Hugging Faceのchat template APIですら、`documents` を渡してRAG用の文書を扱う仕組みを持ってるんだぜ。

つまりユーザーから見ると、

> **このAI、うちの社内情報を知ってるんだぜ。**

に見えるんだけど、

実際には、

> **モデルが覚えてるんじゃなくて、毎回検索して横に置いてるんだぜ。**

ということも普通にあるんだぜ。

### tool useも重みだけでは完成しないんだぜ

最近のAIは、

```text
Web検索するんだぜ
電卓を使うんだぜ
メールを送るんだぜ
DBを読むんだぜ
コードを実行するんだぜ
```

みたいなことをするんだぜ。

でもモデルが直接メールサーバーへTCP接続してるとは限らないんだぜ。

だいたいは、

```text
モデル
    ↓
「send_emailを使いたいんだぜ」とJSONを出す
    ↓
周辺プログラム
    ↓
本当にAPIを呼ぶ
    ↓
結果をモデルへ戻す
```

なんだぜ。

Hugging Faceのtokenizer / chat templateにも `tools` を渡す仕組みがあり、ツール定義をモデルへ見せられるんだぜ。

でも、

**本当にツールを実行する部分は別のソフトなんだぜ。**

### 安全フィルタも外にいることがあるんだぜ

AIサービスには、

```text
入力を検査するんだぜ
出力を検査するんだぜ
個人情報を消すんだぜ
危険な操作を止めるんだぜ
レート制限するんだぜ
ログを残すんだぜ
```

みたいな処理が付くことがあるんだぜ。

これも全部を重みに学習させてるとは限らないんだぜ。

普通のプログラムとして外側に付いてることもあるんだぜ。

だから、

> **このAIはこういう回答をしないんだぜ。**

と思っても、

それが、

```text
重み由来なんだぜ
system prompt由来なんだぜ
外部filter由来なんだぜ
後処理で消してるんだぜ
```

のどれかは、外から見ただけでは分からないことがあるんだぜ。

### 同じ重みでも「別のAI製品」になるんだぜ

例えば同じ重みを使っても、

```text
製品A
    temperature 0なんだぜ
    RAGありなんだぜ
    Web検索ありなんだぜ
    厳しいfilterなんだぜ
    長いsystem promptなんだぜ

製品B
    temperature 1なんだぜ
    RAGなしなんだぜ
    toolなしなんだぜ
    filter少なめなんだぜ
    別のchat templateなんだぜ
```

なら、

ユーザーから見るとかなり違うAIになるんだぜ。

でも核になってる重みは同じかもしれないんだぜ。

つまり、

**AIの振る舞い = 重みだけ**

ではないんだぜ。

### モデルとAIソフトは分けて考えた方がいいんだぜ

ざっくり分けるなら、

```text
モデル
    architectureなんだぜ
    weightsなんだぜ
    tokenizerなんだぜ
    configなんだぜ

推論部分
    runtimeなんだぜ
    GPU kernelなんだぜ
    quantizationなんだぜ
    samplingなんだぜ
    cacheなんだぜ

アプリ部分
    system promptなんだぜ
    chat templateなんだぜ
    RAGなんだぜ
    toolsなんだぜ
    safety filterなんだぜ
    retryなんだぜ
    loggingなんだぜ
    UIなんだぜ
```

みたいに見た方が分かりやすいんだぜ。

もちろん境界は実装によって違うんだぜ。

GGUFみたいにtokenizerやchat templateのmetadataまで一つのファイルへ詰める形式もあるんだぜ。

でも一つのファイルに入ってるからといって、

**全部が「重み」になるわけじゃないんだぜ。**

### だから「重みを公開した = AIソフト全部を公開した」でもないんだぜ

ここも重要なんだぜ。

重みを公開すると、

> **モデルの重要な部分を公開したんだぜ。**

とは言えるんだぜ。

でも実際のサービスが、

```text
どんなsystem promptなんだぜ
どんな検索をしてるんだぜ
どんなtoolを使ってるんだぜ
どんなfilterなんだぜ
どんなsamplingなんだぜ
どんなretryをしてるんだぜ
```

まで同じかは別なんだぜ。

だから重みだけ手に入れて、

> **これであのAIサービスと完全に同じなんだぜ。**

にはならないんだぜ。

### だからなんだぜ

AIを見て、

> **このAIの正体はこの10GBの重みファイルなんだぜ。**

と思うと、半分しか見えてないんだぜ。

重みは確かに中心なんだぜ。

でも実際にユーザーが触ってるAIは、

```text
重みなんだぜ
+
tokenizerなんだぜ
+
推論エンジンなんだぜ
+
chat templateなんだぜ
+
samplingなんだぜ
+
system promptなんだぜ
+
RAGなんだぜ
+
toolなんだぜ
+
filterなんだぜ
+
周辺スクリプトなんだぜ
```

なんだぜ。

**AIソフトってのは、重みのことじゃないんだぜ。**

**重みを中心に、周りの普通のプログラムが山ほどくっついて一つのAIになるんだぜ。**

### Sources

- Hugging Face Transformers, *Writing a chat template*  
  https://huggingface.co/docs/transformers/chat_templating_writing
- Hugging Face Transformers, *Tokenizer*  
  https://huggingface.co/docs/transformers/main_classes/tokenizer
- ggml-org, *llama.cpp — LLM inference in C/C++*  
  https://github.com/ggml-org/llama.cpp
- llama.cpp server documentation, *tokenize, detokenize and chat templates*  
  https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md
