---
title: Computer Architecture论文阅读笔记（5）
date: 2022-07-08 16:15:48
tags:
- computer architecture
categories:
- article reading note
---
**Freeway: Maximizing MLP for Slice-Out-of-Order Execution**

**Abstract**

利用**memory level parallelism (MLP)**对于隐藏长内存和最后一级cache延迟有重要作用。out-of-order core在利用MLP上是有效的，但是能耗比却十分糟糕。因此，我们使用了一重较低复杂度/功耗的机制来利用MLP。

slice-out-of-order cores与full OoO cores相比虽然在功耗上较低，但是在提取MLP方面大幅下降。为了提高sOoO cores中的MLP生成骂我们引入了*Freeway*，一种基于新的dependence-aware slice execution policy的sOoO core，能够跟踪dependent slices，并且将它们作为MLP的提取方式。

<!-- more -->

**INTRODUCTION**

虽然cache层级能够在L1 cache上提供较低的延迟，但是其复杂性使得访问last level cache时花费的40~60个周期成为瓶颈。因此，通过重叠内存访问来将later requests隐藏在之前的requests的“阴影”中，对于性能至关重要。

虽然sOoO cores高度节能，但是在OoO执行后，提取MLP方面会显著下降。在我们的观察中发现，inter-slice dependencies限制了提取MLP的机会。例如，当独立的memory slice到达了顺序B-IQ的头部时，它会阻塞未来的MLP生成，通过停止执行后续可能独立的slice直到load指令的producer获得了数据。总而言之，dependent slices是LSC中的一个严重的瓶颈。

我们建议放弃FIFO模型，使用dependence-aware切片调度模型。模型跟踪硬件中的切片依赖，以识别依赖的切片并将它们和独立的切片分开。为了达到目标，我们提出了Freeway，以最小的额外硬件跟踪切片间依赖，Register Dependence Table中的每个条目中的一位，以筛选依赖切片。这些切片将会被发送到一个新的in-orde queue，**yielding queue (Y-IQ)**，直到它们的producer执行完毕。



**BACKGROUND AND MOTIVATION**

*A. MLP vs Energy: IO, OoO, and slice-OoO cores*

**MLP Extraction**

![](https://pic3.zhimg.com/80/v2-d7ae060b8c23803f05a6f47121c11f6e_720w.jpg)

在图1中，slice S2暂停B-IQ并延迟下一个独立slice S3的执行，直到其producer slice S1接收到来自存储器层次结构的数据。这种切片依赖限制了 MLP 和整体性能。然而，允许slice之间完全out-of-order的sOoO core能够在MLP的生成上与一个完全OoO core相匹配。

**Energy Consumption**

LSC 在IO内核上（ARM Cortex-A7）上仅产生15%的面积和22%的功耗，与in-order core相比 OoO core（ARM Cortex-A9）需要2.5x的面积和12.5x的功耗。

*B. Potential for MLP extraction*

![](https://pic4.zhimg.com/v2-391f33bd977763182c3e69feb18fbc47_r.jpg)

总的来说，大部分的workload说明了如果我们消除了dependent slice的瓶颈，就能够获得可观的性能。

*C. Sources of stalls in the bypass queue*

* **Slice Dependence Stalls**：一个处于B-IQ的头部的dependent slice等待它的producer从memory hierarchy接收数据
* **Empty B-IQ Stalls**：B-IQ中没有指令
* **Load-store Aliasing Stalls**：如果A-IQ中的旧的store正在等待写入相同的地址时，B-IQ头部的load无法被发射
* **Other Stalls**：slice内部依赖，未解析的的store地址阻塞更新的loads

![](https://pic3.zhimg.com/v2-ebca2bfe1399deb55a965dbf9741eef2_r.jpg)

图3显示指令发射平均有 47% 的执行时间被停顿，而Slice Dependence停顿几乎占这些停顿的一半。图4中，`gcc`和`mcf`中大部分的producer slice会在cache中miss，必须从内存中加载。这种长内存延迟在暂停了指令的退休，会阻塞进一步的MLP生成并限制性能。

完全的OoO B-IQ与LSC的IO B-IQ相比，性能提升约20%。IO B-IQ的损失大部分来自于Slice Dependence Stalls，其中B-IQ被相关slice阻塞。



**ADDRESSING SLICE DEPENDENCE**

处理dependent slice的通用方法是将它们在B-IQ之外缓冲，来让它们远离independent slice。

*A. Which dependent slices to buffer?*

缓冲所有相关切片非常重要，即使是那些仅在 L1 命中期间停止的切片。我们只需要将相关切片缓冲几个周期（以覆盖 L1 延迟）即可获得 MLP 的大部分优势。 如果这种有限的缓冲足够，这表明我们可以以较低的成本实现这些好处。

*B. Where to buffer?*

FIFO由于其低复杂度和功耗，是一个不错的指令缓存的选择。然而，如果年轻的slice在旧的slice之前就绪，FIFO会造成瓶颈。我们分析了潜在的停顿源以确定单个、廉价的 FIFO 队列是否适合缓冲相关切片。

* **Slice dependence depth**：在它们的依赖链中进一步分片可能会阻碍更年轻的链中分片的执行。例如图5中，如果所有的slice都会在相同的内存层级中命中，它们的执行时间将会相似，年轻的slice S5将会在S3执行前就绪。然而，它必须暂停在S3后面，因此限制了MLP extraction。图6显示，78%的slice是independent slice（depth 0），因此无需缓冲，在需要缓冲的剩余切片中，超过 72% 处于dependence depth 1。因此，这些由较大深度slice造成的stall可能性较小。

![](https://pic2.zhimg.com/80/v2-cca870d2d85985e9c94b3d46122c41c9_720w.jpg)

![](https://pic1.zhimg.com/v2-c9255aa34fdbba6d3c8e8f38cce42f3c_r.jpg)

* **Producer slice hit site**：即使dependent slices处于相同的depth，如果younger slice的producer比older slice的producer在内存层级中命中了更靠近core的层级，也会导致younger slice先就绪。图7显示，超过96%的producer slice命中了L1 cache，因此该类stall数量极少。

![](https://pic2.zhimg.com/v2-ba932474902927e261312a361d25f905_r.jpg)

综上所述，使用单个FIFO queue来缓存dependent slice是完全足够的。



**FREEWAY**

![](https://pic1.zhimg.com/v2-5b42fab89d39b7893c529db99af089e0_r.jpg)

*Freeway: Overview*

为了使slices乱序执行，Freeway在硬件中跟踪了slice dependence，并且将dependent slice和independent slice分离。为了处理dependent slice，Freeway引入了一个FIFO instruction queue，称为**yielding queue (Y-IQ)**，存放在其中的dependent slice将会等待independent slice的执行直到它们就绪。Freeway扩展了RDT项，增加了一个位用于跟踪指令是否属于一个dependent slice；为dependent slice增加了一个Y-IQ；使用 7 位和比较器扩展每个store buffer entry以保持内存排序；添加逻辑以从 Y-IQ 发出指令。



*Freeway: Details*

*Tracking dependent slices*：我们认为一个memory slice是dependent slice的依据是其是否包含一个older slice的load instruction的指令。通过利用现有的data dependence分析，Freeway能够在Register Renaming阶段检测dependent slice。dependent slice的第一条指令可以使用data dependence分析轻松识别，同时依赖信息必须从第一条依赖指令传播到终止slice的存储器访问指令。

Freeway在RDT中扩展了一个slice dependence bit用于传播slice内的dependence信息，该位只是了指令是否属于一个dependent slice。

*Instruction Flow Through Freeway*：

* **Front-end**：与 LSC 一样，在指令获取和预解码之后，使用指令指针访问IST以检查指令是否属于memory slice。该信息沿流水线传播以协助指令调度。接下来，register renaming识别指令之间真正的数据依赖关系，以便依赖指令等待它们的producer完成执行。这时，Freeway查询RDY确定是否为dependent slice并将信息传递到dispatch stage。
* **Instruction Dispatch**：被IST识别出来的load，store，address-generating指令如果属于independent slice，将被发送到B-IQ，反之发送到Y-IQ。剩下的指令发送到A-IQ。
* **Back-end**：为了跟踪足够数量的指令，Freeway和LSC增加了记分板的大小，超过了in-order core中的典型大小。

*Memory ordering*：

LSC维护内存次序的机制无法直接应用于Freeway。Freeway将所有的load和store按照程序次序的顺序标记。除此以外，在dispatch阶段，store在store buffer中被分配了一个项，该项将在store地址可用时被更新。只有在没有未解析的和aliasing stores的情况下，load才会继续执行。



**EVALUATION**

![](https://pic1.zhimg.com/v2-75ee6a8dd51a40e748060b4bf0131f78_r.jpg)

![](https://pic4.zhimg.com/v2-2effbbb694cd04f87407eb191a2e889f_r.jpg)



**参考资料**：

R. Kumar, M. Alipour and D. Black-Schaffer, "Freeway: Maximizing MLP for Slice-Out-of-Order Execution," 2019 IEEE International Symposium on High Performance Computer Architecture (HPCA), 2019, pp. 558-569, doi: 10.1109/HPCA.2019.00009.