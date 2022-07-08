---
title: Computer Architecture论文阅读笔记（7）
date: 2022-07-08 16:19:15
tags:
- computer architecture
categories:
- article reading note
---
**I-SPY: Context-Driven Conditional Instruction Prefetching with Coalescing**



**Abstract**

为了解决频繁的cache miss，我们提出了一种新颖的prefetching技术，**I-SPY**。在I-SPY中有两种关键的技术：(1) *conditional prefetching*，仅在已知程序上下文会导致未命中时预取指令；(2) *prefetch coalescing*，将多个非连续cache lines的预取合并进一个单独的预取指令。

<!-- more -->

**INTRODUCTION**

I-SPY能够仔细地识别出I-cache miss，将“code prefetch”指令在连接时注入到程序合适的位置，并且在运行时有选择地执行被注入的预取指令。

*Conditional prefetching*

我们在link time时，将预取指令注入到代码中来包含每一个miss。在run time时，我们通过仅在再次观察到miss-inducing context时执行被注入的预取指令来减少不必要的预取。为了实现这个功能，我们首先利用了Intel的*Last Branch Record（LBR）*来确保一个服务器能够依据成功预取的可能性来有选择性地执行被注入的预取指令。我们还提出了一个“code prefetch“指令，称为`Cprefetch`，在其操作数中保存了miss-inducing context的信息，用于保证I-SPY-aware CPU能够有条件地执行预取指令。

*Prefetch coalescing*

我们发现一些程序在面临来自非连续cache lines的I-cache miss时，即在未命中后的N行窗口中，只有N行的一部分会发生miss。我们提出了一个新的指令，叫做`Lprefetch`，用于在一条指令中预取这些非连续的cache line。



**UNDERSTANDING THE CHALLENGES OF INSTRUCTION PREFETCHING**

*A. What Information is Needed to Efficiently Predict an I-Cache Miss?*

 一个应用的执行可以被表示为一个动态的**Control Flow Graph (CFG)**。在CFG中，节点代表了基本块（一系列没有跳转的指令），边代表跳转。为了有效生成动态CFG，我们建议使用Intel’s Precise Event Based Sampling (PEBS) performance counter收集的L1 I-cache miss配置文件来增强动态CFG跟踪。使用这种轻量级监控生成动态CFG可以在生产中分析应用程序。

![](https://pic3.zhimg.com/80/v2-666bfa3410a6dc2bfee025b91d29d30a_720w.jpg)

*B. When To Prefetch an Instruction?*

预取应当在一个合适的窗口中被注入。G. Ayers, N. P. Nagendra, D. I. August, H. K. Cho, S. Kanev, C. Kozyrakis, T. Krishnamurthy, H. Litz, T. Moseley, and P. Ranganathan,
“Asmdb: understanding and mitigating front-end stalls in warehouse-scale computers,” in Proceedings of the 46th International Symposium on Computer Architecture, 2019, pp. 462–473.的工作提出了一种理想的预取窗口，使用average application-specific IPC来注入一条预取指令使cache miss被隐藏。

*C. Where to Inject a Prefetch?*

为了明确预取的有效性，我们对injection site的*fan-out*进行分析。我们将*fan-out*定义为一个已有的injection site中的不会导致target miss的路径比例。通过限制预取注入到那些fan-out低于一定阈值的节点，我们可以提高精度，但是覆盖范围却被减少了。

*D. How to Sparingly Prefetch Instructions?*

增加代码的footprint可能会污染I-cache并且造成一些不必要的cache line eviction，最终降低了程序的性能。因此，谨慎地预取指令以最小化代码占用空间至关重要。**Prefetch coalescing**，我们得出结论，通过单个预取指令对不连续但在空间上邻近的I-cache未命中进行预取合并可以提高性能，同时最大限度地减少静态和动态预取指令的数量。

**I-SPY**

I-SPY提出了*conditional prefetching*来解决高覆盖率和准确度之间的矛盾，用*prefetch coalescing*来减少静态代码的footprint增长。

*A. Conditional Prefetching*

* **Miss context discovery**：I-SPY仅仅考虑在最近的context history中出现的一些重要basic blocks。I-SPY通过识别*predictor
  basic blocks*（导致每次未命中的执行路径中出现频率最高的块）来启动miss context discovery。由于I-SPY仅依赖于blocks的存在来识别context（而不是依赖于块的顺序），它计算predictor blocks的组合作为给定未命中的潜在contexts。然后，I-SPY计算了特定block中导致miss的每一个context的条件概率。I-SPY然后选择概率最高的组合作为给定未命中的context。在run time，如果branch history包含记录的context，conditional prefetch将会被执行。

![](https://pic3.zhimg.com/80/v2-cb0892378678a3854dbe0982e1ab4b06_720w.jpg)

* **Conditional prefetch instruction**：`Cprefetch`需要一个额外的操作数用于指定执行context。context中的每一个basic block是被其第一条指令的地址识别出来的。I-SPY受用LBR data来计算basic block address。I-SPY使用hash来将地址压缩成一个n字节的立即数（*context-hash*）。当`Cprefetch`在运行时执行时，处理器使用最后 32 个前驱basic blocks（英特尔LBR提供 32 个最近执行的基本块的地址）重新计算哈希值（*runtime-hash*），并且将其与context-hash相比较。预取操作只有在context-hash中的set-bits是runtime-hash中的set-bits子集时才会执行。
* **Micro-architectural modifications**：由于LBR是一个 FIFO，我们以增量方式维护runtime-hash。使用Bloom filter，我们为runtime-hash的16位中的每一个分配一个6位计数器（总共96位）。 每当向LBR中添加新条目时，我们都会对相应的块地址进行哈希处理，并在runtime-hash中增加相应的计数器； 被驱逐的LBR条目的哈希计数器递减。计数器永远不会溢出，并且runtime-hash精确地跟踪 LBR 内容，因为在runtime-hash中只记录了32个分支。我们还添加了少量逻辑以将每个计数器减少到单个“is-zero”位； 在这16位中，我们context-hash位是否是runtime-hash的子集。如果是，则预取触发，否则将被禁用。

![](https://pic2.zhimg.com/v2-b751c5814d9fff508a2e09f51f7d30b1_r.jpg)

*B. Prefetching Coalescing*

为了实现coalescing，I-SPY分析了所有被注入到basic block中的预取指令并且将它们按context分组。接着I-SPY尝试去将一组预取指令合并进一条单独的预取指令。I-SPY使用n位的位图在n个连续的cache lines的窗口内选择cache line的子集。

**Coalesced prefetch instruction**。我们提出的coalesced预取指令，`Lprefetch`，需要一个额外的操作数用于指定一个coalescing bit-vector。在现有的硬件中，预取指令根据格式，`(prefetch, address)`来取得*address*作为操作数并预取相应的cache line。

I-SPY通过另一条指令`CLprefetch`，来合并prefetch coalescing和conditional prefetching，其具有格式`(prefetch, address, context-hash, bit-vector)`。只有现在的context和context-hash中的context相匹配时，`CLprefetch`才会预取由bit-vector指定的所有预取目标。

**Micro-architectural modifications**。Coalesced prefetch instruction需要进行微小的微架构修改，主要由一系列简单的incrementers组成。这些incrementers将8-bit的coalescing vector解码并支持预取最多9个cache line。

**Replacement policy for prefetched lines**。I-SPY的预取指令为预取的cache line分配一个优先级，该优先级等于最高优先级的一半。I-SPY使用此策略的目标是减少可能不准确的预取操作的不利影响。

![](https://pic3.zhimg.com/80/v2-4c3c5407a078914cf62abc7d710b4c8e_720w.jpg)



**参考资料**：

T. A. Khan, A. Sriraman, J. Devietti, G. Pokam, H. Litz and B. Kasikci, "I-SPY: Context-Driven Conditional Instruction Prefetching with Coalescing," 2020 53rd Annual IEEE/ACM International Symposium on Microarchitecture (MICRO), 2020, pp. 146-159, doi: 10.1109/MICRO50266.2020.00024.

