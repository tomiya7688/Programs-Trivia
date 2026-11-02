# #003 Rustってな、実は完全なメモリ安全じゃないんだぜ

Rustといえば **メモリ安全** なんだぜ。

所有権、借用、ライフタイム、境界チェックなんかで、CやC++でありがちなメモリ破壊をかなりのところまで防いでくれるんだぜ。

特に **Safe Rustだけを書いている限り、メモリ安全なんだぜ** と説明されることが多いんだぜ。

でもな。

現実に動くプログラムは、Rustの型システムだけで出来ているわけじゃないんだぜ。

```text
Rustのコード
    ↓
rustc
    ↓
LLVM
    ↓
機械語
    ↓
CPU
```

なんだぜ。

つまり、Rust側でどれだけ安全性を証明しても、**その下でコンパイラが間違った機械語を作ったら終わりなんだぜ。**

### Safe Rustなのにsegfaultしたことがあるんだぜ

2020年のRust 1.41.0では、実際に **Safe RustのコードがLLVMのmiscompileによってsegfaultする** バグが報告されたんだぜ。

Issueのタイトルもそのまんま、

> **Memory unsafety problem in safe Rust**

なんだぜ。

問題のプログラムでは、範囲外のsliceアクセスをしようとしていたんだぜ。

普通ならRustの境界チェックが働いて、

```text
index out of bounds
```

でpanicするはずなんだぜ。

つまりプログラムとしては間違っていても、**メモリの外を勝手に読む前にRustが止める**はずなんだぜ。

ところが最適化を有効にしたRust 1.41.0では、sliceの長さがおかしな値になり、境界チェックまで消えて、最後はsegmentation faultしたんだぜ。

Safe Rustなのにだぜ。

### 犯人はLLVMの最適化だったんだぜ

調査すると、Rustの型チェックやborrow checkerが間違っていたわけじゃなかったんだぜ。

Miriでは問題なく、最適化前の段階でも筋は通っていたんだぜ。

壊れたのは、その後なんだぜ。

LLVM 9の **Scalar Evolution / Induction Variable Simplification** 周辺の最適化で、本当は保証できない計算に対して

```text
nuw
```

つまり **「unsigned overflowしないんだぜ」** という印を付けてしまうバグがあったんだぜ。

LLVMはその印を信じて、

「じゃあこの条件はこうなるはずなんだぜ」

と最適化を進めた結果、本来必要だった境界チェックまで消してしまったんだぜ。

**コンパイラは、嘘の前提を1個信じるだけで安全装置まで消せるんだぜ。**

### しかもLLVM側ではもう直ってたんだぜ

このLLVMの変更はLLVM 9に入っていたんだぜ。

でも問題が見つかって、LLVM本家では後にその変更がrevertされていたんだぜ。

LLVM 10では修正済みだったんだぜ。

ところがRust 1.41.0はLLVM 9系のsnapshotを使っていたので、そのバグを踏んだんだぜ。

RustチームはRust 1.41.1でLLVM側のrevertをcherry-pickして修正したんだぜ。

Rust公式の1.41.1リリース記事にも、

**LLVMの最適化によって境界チェックが誤って消え、segfaultしていた**

とちゃんと書かれているんだぜ。

### じゃあRustはメモリ安全じゃないのか、なんだぜ

ここが大事なんだぜ。

Rustの **言語としてのSafe Rustのルール** は、メモリ安全になるように設計されているんだぜ。

でもその保証を現実の機械まで持っていくには、

```text
型システムが正しい
rustcが正しい
LLVMが正しい
標準ライブラリのunsafe実装が正しい
CPUが正しく動く
```

みたいな前提が必要なんだぜ。

だから、

> **Safe Rustなら、どんなコンパイラバグがあっても絶対にメモリ事故は起こらないんだぜ**

ではないんだぜ。

より正確には、

> **Safe Rustは、正しい処理系の上ではメモリ安全になるように設計されているんだぜ。**

なんだぜ。

この話の面白いところは、

**安全な言語でも、最後には安全じゃないくらい巨大な下の層を信用してるんだぜ**

ってところなんだぜ。

### Sources

- Rust Blog, *Announcing Rust 1.41.1*  
  https://blog.rust-lang.org/2020/02/27/Rust-1.41.1/
- rust-lang/rust #69225, *Memory unsafety problem in safe Rust*  
  https://github.com/rust-lang/rust/issues/69225
- LLVM commit reverting the faulty SCEV optimization  
  https://github.com/llvm/llvm-project/commit/58e8c793d0e43150a6452e971a32d7407a8a7401
