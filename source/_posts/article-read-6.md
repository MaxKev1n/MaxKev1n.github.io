---
title: Computer Architecture论文阅读笔记（6）
date: 2022-07-08 16:17:55
tags:
- computer architecture
categories:
- article reading note
---
**The Forward Slice Core Microarchitecture**

**ABSTRACT**

我们提出了一种新颖的core microarchitecture，**Forward Slice Core (FSC)**，建立在stall-on-use in-order core的基础上，并且获得了比slice-out-of-order cores更多的instruction-level and memory-hierarchy parallelism。FSC识别和发送forward slice到专用的in-order FIFO queue。除此以外，FSC将依赖于L1 D-cache miss的load-consumer放到一边来让更新的independent load-consumer更快执行。最终，FSC通过跨队列复制存储地址指令消除了动态内存消歧的需要。

<!-- more -->

**INTRODUCTION**

在dispatch阶段，所有不属于forward slice的指令将会被传送到一个in-order FIFOqueue，叫做**Main Lain（ML）**，这使得与older load指令独立的其他指令可以尽快执行。forward-slice指令将会被专用的FIFO queue：load将会被送入**Dependent Load Lane (DLL)**，非load指令将会被放入**Dependent Execute Lane (DEL)**。在DEL头部等待超过预先设置的周期的指令将会被送入**Holding Lane (HL)**来确保与L1 D-cache miss独立的指令能够被执行。

FSC和之前的工作相比有三个不同：

1. FSC在forward slice上操作而不是backward slice上，这简化了硬件。
2. FSC 将等待L1 D-cache未命中的所有DEL指令排空到单独的HL。这些指令在in-order core中导致最长的性能停顿，并且将它们重新定向到单独的队列可以加速年轻独立指令的执行。
3. FSC通过**store-address replication（SAR）**来完成memory disambiguation。当dispatch store指令时，FSC将store-address micro-op复制到所有FIFO queue。只有当所有复制的micro-op在它们的queue头部时，store-address micro-op才会被发射到functional unit，这保证了load不会绕过older store。



**BACKGROUND AND MOTIVATION**

*Shortcomings of Slice-Out-of-Order Cores*

sOoO core有三个主要缺点被我们解决：

* **Limited Instruction-Level Parallelism**：younger independent instructions可能会被load-consumer阻塞。尤其是，一条等待load从memory hierarchy中返回结果的指令将会暂停A-queue头部多个周期。

* **Limited Memory-Hierarchy Parallelism**：sOoO core中的MHP仍然被至少两个原因限制。首先，sOoO core依赖IBDA去识别AGIs，如果workload中的AGIs超过了IST的大小，将会导致IST miss，使AGIs被发送到A-queue。第二，Y-queue中被暂停的dependent slice将会导致队列中younger dependent slice无法被执行。

* **Hardware Complexity**：首先，sOoO需要专用硬件来动态地计算backward slice。IST需要$N$个读端口和$N$个写端口来支持$N$ wide superscalar pipeline，且需要在一个时钟周期内访问。这对宽和高频流水来说是个挑战。第二，为了保证所有的memory dependence正确，Freeway需要用连续数字来标记load和store。只有没有为解析的和aliasing的store时，load才能被处理。这个操作需要：1. 将load的数字与store buffer中的所有store进行对比；2. 内存地址的关联比较。



**FORWARD SLICE CORE**

*Identifying Forward Slices*

我们将依赖（直接或间接）一条load指令的一系列指令定义为forward slice。微架构使用一个叫做**Steering Bit Vector (SBV)**的bit vector硬件来识别forward-slice指令。在寄存器重命名一条load指令时，将SBV中对应目的物理寄存器的一位设置。younger指令读取一个物理寄存器时，如果发现对应的SBV位被设置了，他的目的物理寄存器对应的位也被设置。当一个条指令执行并且计算出了目的物理寄存器值时将对应的SBV位清除。

*Instruction Steering*

non-forward-slice指令将会被发送到*Main Lane（ML）*。forward-slice中的load指令将会被发送到*Dependent Load Lane（DLL）*中，剩余的其他forward-slice指令将会被发送到*Dependent Execute Lane（DEL）*中，这种设计能够保证younger independent non-load指令能够在older load-dependent load指令前执行。当一个lane满了，dispatch将会停止，并且back-pressure导致剩余的前端流水线被停止。

*Holding Lane*

基本的思想是将属于L1-missing load的forward-slice的指令移动到HL，使younger independent forward-slice指令更早地执行。当一条在DEL头部的指令的producer在L1 D-cache中发生load miss时被移动到HL中，为了实现它，我们设置了一个值被预先设置的counter。counter中的值每周期都会递减，当达到0时，这标志着出现了L1 D-cache miss，并且将DEL头部指令移动到HL中，然后counter值重新被设置为预定的值。

*Store-Address Replication*

我们提出了**Store-Address Replication（SAR）**做为一个用于解决*memory disambiguation problem*的简单但是有效的方法。SAR可以应用到任意multi-queue架构中，包括FSC。SAR在指令传输时将*store-address (STA)* micro-op传送到4个lane中。当一个STA micro-op处于所有lane的头部并且operand全部可用时将会被选中执行。FSC从ML中执行STA micro-op，并且将其余的重复副本删除。

*Code Example*

![](https://pic3.zhimg.com/80/v2-d11f1ead91b051daeca495c0bc429606_720w.jpg)



**EXPERIMENTAL SETUP**

![](https://pic2.zhimg.com/80/v2-eb423e25c33b3bbc73bc945d2836940d_720w.jpg)

![](https://pic4.zhimg.com/v2-e2c550eb162f9d260a289bac4015a32b_r.jpg)



**参考资料**：

*Kartik Lakshminarasimhan, Ajeya Naithani, Josué Feliu, and Lieven Eeckhout. 2020. The Forward Slice Core Microarchitecture. In* *Proceedings of the ACM International Conference on Parallel Architectures and Compilation Techniques* *(**PACT '20**). Association for Computing Machinery, New York, NY, USA, 361–372. DOI:https://doi.org/10.1145/3410463.3414629*

