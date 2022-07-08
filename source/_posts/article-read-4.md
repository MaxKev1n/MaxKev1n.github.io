---
title: Computer Architecture论文阅读笔记（4）
date: 2022-07-08 16:14:29
tags:
- computer architecture
categories:
- article reading note
---
**The Load Slice Core Microarchitecture**

**Abstract**

off-chip memory wall和由于多核处理器的复杂cache层级带来的访存和访问cache花费增加，提高了**memory hierarchy parallelism (MHP)**的重要性，减少了更通用但复杂且耗电的 ILP 提取技术的净影响。除此以外，当多核处理器运行在功耗限制的环境下，节能会代替单线程性能成为主要关注点。因此，我们提出了一个专注于并行访问内存层级时最小化功耗的内核微架构。Load Slice Core使用了第二个顺序流水线来保证访存和地址生成指令能够绕过主流水线的暂停的指令。

<!-- more -->

**Introduction**

我们提出了 Load Slice Core 微架构，它是一种受限制的乱序机器，旨在从内存层次结构中提取并行性。Load Slice Core微架构既有两条in-order流水线：一条主流水线用于指令流，一条用于处理loads和地址计算。

* 我们提出了*iterative backward dependency analysis*，这是一种低成本、基于硬件的技术，用于从加载和存储指令中选择backward指令片以供早期执行。

* 我们提出了 Load Slice Core 微架构，一种受限制的乱序、解耦的访问/执行式微架构。能够根据iterative backward dependency analysis，在流水线前端的早期进行调度决策。



**Motivation**

* **Out-of-order loads**： out-of-order loads架构能够获得额外的MHP，但是ILP是相同的。但是，与顺序执行相比能够获得更好的性能，这个架构能够更早地发射load。这减少了指令用于处理load结果的stall时间，更重要的是能够使load绕过由于等待处理之前的load而阻塞窗口的指令，允许更多的loads被并行发射。

* **Address-generating instructions**：我们将地址生成指令（Address-generating instructions）定义为仍在指令窗口中的任何指令，从该指令到加载地址（可能跨控制流）存在依赖链。假设架构完全了解需要哪些指令来计算未来的加载地址，并使它们都能乱序执行。 这反过来会更早地生成加载地址，从而使更多的load也可以乱序执行，进一步提高性能，达到接近完全乱序执行的水平。

* **In-order scheduling**：调度策略可以通过使用两个in-order queue来实现，一个*bypass queue*用于loads和AGIs，另一个*main queue*用于其他所有的指令。这个设计的性能比in-order core高53%，与完全无序执行的内核相比，性能提升了 11%。我们并不是尝试在一个执行过程中获得完成load address的完成依赖链，而是一次找到一个producer，并且mark instructions as address-generating one backward step per loop iteration。只有那些在之前的迭代中已经被标记为 AGI 的指令才会被发送到旁路队列，从而大大简化了将指令分派到正确队列所需的逻辑。
* **Key insights**：为了检测load和更早的store指令之前的依赖，我们将store指令分为两部分，一部分计算地址，另一部分收集数据并更新内存。地址部分使用bypass queue，而数据部分在main queue中执行。

![](https://pic2.zhimg.com/v2-e4f4c305ac8d312fa40ccdeb825ae799_r.jpg)



**Iterative Backward Dependency Analysis**

*iterative backward dependency analysis (IBDA)*的目的是为了以低成本、硬件有好的方式识别生成地址的指令。通过利用程序的自然循环行为，我们可以在随后的循环迭代中一次找到一条指令的完整backward slice。每一条被发现的指令都被标记为一个address-generating slice的一部分，并且它的producer将会在下一个循环迭代中被标记。

IBDA的实现需要增加两个新的硬件结构。*The instruction slice table (IST)*，包含了已经被识别属于一个backward slice的指令地址。通过在dispatch阶段使用IST中的数据，Load Slice Core可以确定指令是否已经被标记：存在于IST中的指令会被插入到bypass queue。第二个结构是*register dependency table (RDT)*，为每一个物理寄存器存放了一个项，并且映射到最后一个写该寄存器的指令指针。RDT将被用来查询之前产生地址计算必需的寄存器的指令。这些指令被认为是AGIs，其指令地址被记录在IST中。



**The Load Slice Core Microarchitecture**

![](https://pic2.zhimg.com/80/v2-a9601d2ef32797748dd0f0f78fcc8c49_720w.jpg)

* **Front-end pipeline**：从Icache中取出指令后，在IST中查询该指令是否为address-generating指令，并生成一个IST命中位反映是否在IST中。
* **Register renaming**：在Load Slice Core中，寄存器重命名除了用于消除依赖外，也用于简化两个instruction queue的交互。通过使用寄存器重命名，可以提前计算来自bypass queue的结果，存储在register file中，然后由bypass或main queue引用。

* **Dependency analysis**：IBDA使用两个结构：IST和RDT。IST就像一个cache tag array一样，IST中存放所有被识别为address-generating指令的地址，并且不存放数据。RDT用于识别指令间的依赖，每一个RDT中的物理寄存器包含了上一个写入这个寄存器的指令地址。如果现指令是一个load，store或者被标记的address generator，它的所有producer能从RDT中找到，如果producer的IST位未被设置，那么该producer的地址会被插入到IST中。

* **Instruction dispatch**：指令被分派到适当的队列中，要么根据它们的类型（加载/存储），要么根据它们的IST位。 load指令总是插入到 B queue中。 如果address-generating指令在取指时存在于IST中，则它们将进入 B queue。store进入两个队列：存储地址计算从 B queue执行，以便未解析的存储地址自动阻塞未来的load； 同时从 A queue中收集存储数据，使存储仅在确保没有遇到异常之后才能按照程序顺序继续更新内存。所有其他指令都被分派到主队列中。

* **Issue/execute**：由于address-generating指令通常由简单的算术运算组成，因此B流水线的execution cluster可以由内存接口和简单的ALU组成。为了防止复杂的address-generating指令进入B queue，前端会根据opcode将复杂的address-generating指令插入到A queue，即使IST位被设置。

* **Memory dependencies**：存储数据从main queue中获得，而load和store所有的地址计算都通过bypass queue处理。由于bypass queue是顺序的，因此地址未被解析的store指令会自动阻止未来——可能conflicting的load被执行。当用于计算存储地址的操作数准备就绪，则store从bypass queue中被执行，将地址写入到store buffer中。当store到main queue的头部时，它的数据也可用，并且可以将存储写入内存。

* **Commit**：commit阶段检查异常并使（以前推测的）状态在架构上可见，并释放诸如store buffer entries和rename registers之类的结构。instruction在调度时按顺序输入scoreboard，乱序记录其完成，并按顺序离开scoreboard。



**Conclusion**

基于详细的时序仿真以及面积和功耗的估计，我们证明Load Slice Core比传统解决方案的面积和功耗效率更高：平均性能比in-order, stall-on use core高53%，但面积开销仅高15%，且功耗增加仅22%。这使得基于Load Slice Core的功率和面积受限的多核设计的性能分别优于基于有序和无序的替代方案，分别高出 53% 和 95%。

![](https://pic1.zhimg.com/v2-f7e09f05a49dae37f5b610d490738404_r.jpg)

![](https://pic1.zhimg.com/80/v2-901d5de1e53753ea2c843ef87849b950_720w.jpg)



**参考资料**：

T. E. Carlson, W. Heirman, O. Allam, S. Kaxiras and L. Eeckhout, "The Load Slice Core microarchitecture," 2015 ACM/IEEE 42nd Annual International Symposium on Computer Architecture (ISCA), 2015, pp. 272-284, doi: 10.1145/2749469.2750407.

