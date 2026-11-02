# #018 実はブラウザってな、JavaScript以外も動かせるんだぜ

Webブラウザで動くプログラムって聞くと、

```text
JavaScriptなんだぜ
```

って思うんだぜ。

実際、長いあいだWebブラウザ上で標準的に動くプログラムといえばJavaScriptだったんだぜ。

でもな。

**今のブラウザはJavaScript以外のコードも実行できるんだぜ。**

その代表が **WebAssembly** なんだぜ。

### WebAssemblyって何なんだぜ

WebAssembly、略して **Wasm** は、

> **ブラウザで効率よく実行できる低レベルなコード形式なんだぜ。**

なんだぜ。

W3Cの仕様でも、

```text
safe
portable
low-level code format
```

として定義されてるんだぜ。

つまり、

> **ブラウザ用のもう一つの実行形式なんだぜ。**

くらいに考えると分かりやすいんだぜ。

JavaScriptとは別物なんだぜ。

### RustやC++もブラウザで動かせるんだぜ

ここが面白いところなんだぜ。

WebAssemblyは、人間が直接書くことを主目的にした言語じゃないんだぜ。

むしろ、

```text
Cなんだぜ
C++なんだぜ
Rustなんだぜ
C#なんだぜ
```

みたいな言語の **コンパイル先** として使われるんだぜ。

例えばRustなら、

```text
Rustのコード
    ↓
rustc / wasm-pack
    ↓
WebAssembly
    ↓
ブラウザ
```

なんだぜ。

MDNにも、RustをWebAssemblyへコンパイルして既存のWebアプリで使う公式ガイドがあるんだぜ。

つまり、

> **ブラウザでRustを動かすんだぜ。**

は、かなり普通にできるんだぜ。

### でもブラウザがRustを直接読んでるわけじゃないんだぜ

ここは大事なんだぜ。

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

をブラウザへそのまま渡して、

> **Rustなんだぜ。実行してくれなんだぜ。**

ではないんだぜ。

ブラウザが実行してるのは、そのRustから作られた **WebAssembly** なんだぜ。

つまり、

```text
Rustを実行してるんだぜ
```

というより厳密には、

> **RustからコンパイルされたWasmを実行してるんだぜ。**

なんだぜ。

CやC++も同じなんだぜ。

### Pythonまでブラウザで動くんだぜ

さらに面白いのが **Pyodide** なんだぜ。

Pyodideは、

> **CPythonをWebAssembly / Emscriptenへ移植したものなんだぜ。**

なんだぜ。

だからブラウザの中で、

```python
import sys
print(sys.version)
```

みたいなPythonを普通に実行できるんだぜ。

しかも、

```text
NumPy
pandas
SciPy
Matplotlib
scikit-learn
```

みたいな科学計算系のパッケージまでかなり動くんだぜ。

つまり、

> **ブラウザの中にPython処理系そのものを持ち込むんだぜ。**

なんだぜ。

### 「ブラウザはJavaScriptしか動かせない」はもう違うんだぜ

今のブラウザはざっくり言うと、

```text
JavaScriptを実行するんだぜ
WebAssemblyを実行するんだぜ
```

が標準機能なんだぜ。

そしてWebAssemblyを使うことで、

```text
RustをWasmにするんだぜ
C++をWasmにするんだぜ
C#をWasmにするんだぜ
Python処理系をWasmにするんだぜ
```

みたいなことができるんだぜ。

だから実質的には、

> **ブラウザで動かせる言語の選択肢が一気に増えたんだぜ。**

なんだぜ。

### じゃあJavaScriptはもういらないんだぜ？

そうはならないんだぜ。

WebAssemblyはJavaScriptを置き換えるためだけに作られたものじゃないんだぜ。

MDNも、WebAssemblyはJavaScriptを **補完して一緒に動く** ものとして説明してるんだぜ。

特にDOMなんかのWeb APIは、歴史的にJavaScriptから使う前提が強いんだぜ。

Wasmだけで全部完結するというより、

```text
重い計算はWasmなんだぜ
UIやDOM操作はJavaScriptなんだぜ
両方をつなぐんだぜ
```

みたいな構成がよくあるんだぜ。

Emscriptenも、C/C++からWasmを作るときにJavaScriptのglue codeを生成するんだぜ。

### ブラウザの中に「CPUみたいな実行環境」が増えたんだぜ

WebAssemblyの面白さは、

> **別の言語を無理やりJavaScriptへ変換したんだぜ。**

だけじゃないんだぜ。

ブラウザの中に、

```text
JavaScript engine
WebAssembly runtime
```

という別系統の実行環境があるんだぜ。

Wasmはバイナリ形式で、ブラウザが検証してコンパイルして実行するんだぜ。

だから、

> **WebブラウザはHTMLを見るソフトなんだぜ。**

から、

> **安全なサンドボックスの中で複数の実行形式を動かす仮想マシンみたいなもんなんだぜ。**

にかなり近づいてるんだぜ。

### 昔もJava以外を動かそうとはしてたんだぜ

実は、

```text
Java Appletなんだぜ
Flashなんだぜ
Silverlightなんだぜ
```

みたいに、JavaScript以外をブラウザで動かす仕組みは昔からあったんだぜ。

でもあれは多くの場合、

> **ブラウザにプラグインを入れるんだぜ。**

という方式だったんだぜ。

WebAssemblyが違うのは、

> **ブラウザ標準として実行環境が入ってるんだぜ。**

というところなんだぜ。

追加プラグイン前提じゃないんだぜ。

### だからなんだぜ

昔の感覚だと、

> **ブラウザでプログラムするならJavaScriptなんだぜ。**

だったんだぜ。

今でもJavaScriptがWeb開発の中心なのは変わらないんだぜ。

でも、

> **ブラウザがJavaScriptしか実行できないんだぜ。**

はもう違うんだぜ。

ブラウザはWebAssemblyも動かせるんだぜ。

そのおかげで、

```text
Rustなんだぜ
Cなんだぜ
C++なんだぜ
C#なんだぜ
Pythonなんだぜ
```

みたいな世界までWebへ持ち込めるんだぜ。

**今のブラウザは、JavaScript専用機じゃないんだぜ。**

### Sources

- MDN, *WebAssembly*  
  https://developer.mozilla.org/en-US/docs/WebAssembly
- MDN, *WebAssembly concepts*  
  https://developer.mozilla.org/en-US/docs/WebAssembly/Guides/Concepts
- MDN, *Compiling from Rust to WebAssembly*  
  https://developer.mozilla.org/en-US/docs/WebAssembly/Guides/Rust_to_Wasm
- W3C, *WebAssembly Core Specification*  
  https://www.w3.org/TR/wasm-core/
- Pyodide documentation  
  https://pyodide.org/en/stable/
