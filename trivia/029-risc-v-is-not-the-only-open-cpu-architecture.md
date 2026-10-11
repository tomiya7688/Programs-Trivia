# #029 RISC-VだけがオープンなCPUアーキテクチャじゃないんだぜ

オープンなCPUアーキテクチャって聞くと、

```text
RISC-Vなんだぜ
```

って思うことが多いんだぜ。

今いちばん有名だから当然なんだぜ。

でもな。

**RISC-Vだけが「開かれたCPU」の試みじゃないんだぜ。**

その前にも、その横にも、結構いろんな系譜があるんだぜ。

## まず「何がオープンなんだぜ？」があるんだぜ

CPUの「オープン」って、実は一種類じゃないんだぜ。

例えば、

```text
命令セットの仕様が公開されてるんだぜ
誰でも実装していいんだぜ
RTLのsource codeまで公開されてるんだぜ
toolchainもopen sourceなんだぜ
platform仕様も公開されてるんだぜ
```

は全部違うんだぜ。

だから、

> **open-source CPUなんだぜ。**

と、

> **open ISAなんだぜ。**

は同じ意味じゃないんだぜ。

### RISC-Vは「オープン標準のISA」なんだぜ

RISC-V International自身も、

> **RISC-Vはopen sourceというより、open standard ISAなんだぜ。**

と説明してるんだぜ。

ISAは、

```text
このbit列ならこの命令なんだぜ
registerはこうなんだぜ
例外はこうなんだぜ
memory modelはこうなんだぜ
```

という、

**softwareとhardwareの間の約束**

なんだぜ。

RISC-Vではその仕様がfreeで公開されていて、誰でもその仕様に沿ったprocessorを作れるんだぜ。

でも、

> **RISC-V CPUの回路図が一個あって、それがopen sourceなんだぜ。**

という話ではないんだぜ。

RISC-VというISAに対して、

```text
小さいcoreを作るんだぜ
巨大なout-of-order coreを作るんだぜ
FPGA用に作るんだぜ
ASICにするんだぜ
```

といろんな実装を作れるんだぜ。

## OpenRISCはRISC-Vより前からいたんだぜ

**OpenRISC** っていうarchitectureがあるんだぜ。

OpenRISC 1000、略してOR1Kなんだぜ。

公式自身が、

> **completely open architectureなんだぜ。**

と言ってるんだぜ。

しかも仕様書のrevision historyを見ると、最初のdocumentは **2000年** なんだぜ。

RISC-Vの開始は2010年なんだぜ。

つまり、

> **オープンなRISC ISAをみんなで作るんだぜ。**

という発想自体は、RISC-Vよりずっと前からあったんだぜ。

OpenRISCには、

```text
GCCなんだぜ
GDBなんだぜ
binutilsなんだぜ
Linuxなんだぜ
QEMUなんだぜ
FPGA実装なんだぜ
```

まであるんだぜ。

2025年にもtoolchainやrootfs binaryが更新されていて、今も普通に生きてるprojectなんだぜ。

## OpenPOWERも「誰でもPOWER互換CPUを作れる」方向へ開いたんだぜ

**POWER ISA** もあるんだぜ。

IBM POWERの系譜なんだぜ。

昔は、

> **IBMのarchitectureなんだぜ。**

という印象が強かったんだぜ。

でも2019年、IBMとOpenPOWER FoundationはPOWER ISAをopen licenseで公開する方向へ進めたんだぜ。

OpenPOWER Foundationの説明では、

> **誰でもPOWER ISA compliantなchip、ASIC、FPGAをroyaltyなしで作れるんだぜ。**

としてるんだぜ。

今もPower ISA 3.1cなどの仕様が公開されてるんだぜ。

しかもISAだけじゃなく、

```text
ABIなんだぜ
Linux platform仕様なんだぜ
compliance testなんだぜ
```

までOpenPOWER側で整備されてるんだぜ。

つまり、

> **RISC-Vだけが「オープンなISA ecosystem」なんだぜ。**

でもないんだぜ。

## OpenSPARCはさらに豪快なんだぜ

**OpenSPARC** っていうのもあるんだぜ。

Sun Microsystemsが2006年に **UltraSPARC T1のprocessor designそのもの** をopen sourceにしたんだぜ。

Oracleのarchiveにも今でも残ってるんだぜ。

しかも公開されたのは、

```text
Verilog RTLなんだぜ
verification environmentなんだぜ
diagnostic testなんだぜ
simulation toolなんだぜ
synthesis scriptなんだぜ
hypervisor sourceなんだぜ
architecture documentなんだぜ
```

までなんだぜ。

かなり本気なんだぜ。

OpenSPARC T1のsource codeの大部分はGPLで公開されたんだぜ。

ここはRISC-Vとの違いが面白いんだぜ。

### RISC-V

```text
ISAの仕様をopen standardにするんだぜ
↓
好きなCPU実装を作るんだぜ
```

なんだぜ。

### OpenSPARC

```text
実際にcommercial productとして使ったCPU designのRTLまで公開するんだぜ
```

なんだぜ。

**「何をopenにするか」が違うんだぜ。**

## open ISAとopen implementationは分けて考えるんだぜ

例えば、

```text
ISAはopenなんだぜ
でもCPU coreのRTLは企業秘密なんだぜ
```

も普通にあり得るんだぜ。

逆に、

```text
あるCPU coreのRTLはopen sourceなんだぜ
でも元になってるISAは別のlicenseなんだぜ
```

もあり得るんだぜ。

だからCPUの世界では、

```text
ISAなんだぜ
microarchitectureなんだぜ
RTLなんだぜ
SoCなんだぜ
platformなんだぜ
toolchainなんだぜ
```

を分けて考えると分かりやすいんだぜ。

「architectureがopen」という一言だけでも、

**どの層がopenなんだぜ？**

を見る必要があるんだぜ。

## じゃあなんでRISC-Vだけこんなに有名なんだぜ

ここが#015ともつながるんだぜ。

前に、

> **プログラミング言語は、いい言語だから流行るだけじゃないんだぜ。**

って話をしたんだぜ。

CPU architectureも似てるんだぜ。

OpenRISCはRISC-Vより前からいたんだぜ。

OpenSPARCも実際のcommercial CPU designまで公開したんだぜ。

POWERもopenになったんだぜ。

でも今、

> **open ISAといえばなんだぜ？**

と聞くとRISC-Vが一番出てくるんだぜ。

なぜなんだぜ。

### RISC-Vは最初から「ISAだけをきれいに切り出した」んだぜ

RISC-Vは、

```text
小さいbase ISAなんだぜ
必要ならextensionを足すんだぜ
microarchitectureを縛らないんだぜ
研究にも製品にも使えるんだぜ
```

という思想なんだぜ。

公式manualにも、

> **特定のmicroarchitectureやimplementation technologyへ寄りすぎないんだぜ。**

という設計目標が明記されてるんだぜ。

つまり、

**「このCPUをopen sourceにする」ではなく「CPUを作るための共通言語をopen standardにする」**

にかなり徹したんだぜ。

### ecosystemが育ったんだぜ

RISC-V Internationalの2025 annual reportでは、初期には、

> **openなだけではacademiaにもあまり響かなかったんだぜ。**

という話まで書いてあるんだぜ。

みんな、

> **industryで使われてるものを教えたいんだぜ。**

だったんだぜ。

でも2014〜2015年ごろからindustryの参加が増えて、

```text
compilerなんだぜ
OSなんだぜ
debuggerなんだぜ
CPU coreなんだぜ
SoCなんだぜ
vendorなんだぜ
tool vendorなんだぜ
boardなんだぜ
```

が揃っていったんだぜ。

RISC-V自身も、

> **昔はecosystem maturityが弱点だったけど、今はecosystemとopennessが成功の理由になったんだぜ。**

という状態なんだぜ。

**openであることと、使えるecosystemがあることは別なんだぜ。**

## 「先にあった」だけでも勝てないんだぜ

OpenRISCは2000年からあるんだぜ。

RISC-Vは2010年からなんだぜ。

それでも後発のRISC-Vが圧倒的に知られるようになったんだぜ。

これも、

> **技術は一番早く作ったものが必ず勝つんだぜ。**

じゃない証拠なんだぜ。

```text
設計なんだぜ
licenseなんだぜ
標準化なんだぜ
governanceなんだぜ
toolchainなんだぜ
企業参加なんだぜ
教育なんだぜ
communityなんだぜ
タイミングなんだぜ
```

が全部効くんだぜ。

## だからなんだぜ

RISC-Vを見て、

> **openなCPU architectureという発想をRISC-Vが初めて作ったんだぜ。**

と思ったら違うんだぜ。

OpenRISCがいたんだぜ。

OpenPOWERがいるんだぜ。

OpenSPARCみたいにprocessor RTLまで丸ごとopenにした例もあるんだぜ。

でも同時に、

**RISC-Vが成功したのは、単に「openだったから」だけでもないんだぜ。**

openな仕様の周りに、

```text
人なんだぜ
会社なんだぜ
toolなんだぜ
softwareなんだぜ
実装なんだぜ
標準化なんだぜ
```

が集まったんだぜ。

> **RISC-V以外にもオープンなarchitectureは結構あるんだぜ。**
>
> **でも「openであること」と「広く使われること」は、また別の話なんだぜ。**

なんだぜ。

### Sources

- RISC-V International, *RISC-V: An Open Standard Instruction Set Architecture*  
  https://riscv.org/blog/risc-v-an-open-standard-instruction-set-architecture/
- RISC-V Ratified Specifications, *Introduction*  
  https://docs.riscv.org/reference/isa/v20240411/unpriv/intro.html
- OpenRISC, *Architecture*  
  https://openrisc.io/architecture
- OpenRISC, *Project Overview*  
  https://openrisc.io/
- OpenRISC 1000 Architecture Manual  
  https://openrisc.io/or1k.html
- OpenPOWER Foundation, *Instruction Set Architecture*  
  https://openpowerfoundation.org/specifications/isa/
- OpenPOWER Foundation, *Final Draft of the Power ISA EULA Released*  
  https://openpowerfoundation.org/blog/final-draft-of-the-power-isa-eula-released/
- Oracle, *OpenSPARC Overview*  
  https://www.oracle.com/servers/technologies/opensparc.html
- Oracle, *OpenSPARC T1*  
  https://www.oracle.com/servers/technologies/opensparc-t1-page.html
- RISC-V International, *RISC-V Annual Report 2025*  
  https://riscv.org/wp-content/uploads/2026/01/RISC-V-Annual-Report-2025.pdf
