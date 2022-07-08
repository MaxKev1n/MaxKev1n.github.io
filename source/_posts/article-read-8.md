---
title: Computer Architecture论文阅读笔记（8）
date: 2022-07-08 16:20:52
tags:
- computer architecture
categories:
- article reading note
---
## Abstract

由于I-cache misses，branch mispredictions和指令流中的跳转，通过扩大规模来获取更高的性能是一件具有挑战的事。同时，VLSI互连技术的发展进一步地限制了前端设计地性能。为了解决这些挑战，我们提出了一种fetch架构，称之为**Fetch Target Buffer**，这是一种*multi-level fetch block-oriented predictor*。我们将FTB从取指和译码流水线中解耦出来，使流水线能获得更高的频率。

<!-- more -->

## Introduction

现代高性能处理器由两个processing engine组成，前端处理器和execution core。前端处理器用于取指和准备执行的指令。execution core负责指令的执行、寄存器退休以及内存存储。通常，这些engine由buffering stage连接，比如指令取指队列或者保留站。

这种前端和execution core之间的producer和consumer的关系造成了计算中的重要瓶颈，执行性能被取指性能限制。

不幸的是，扩大前端的性能不是容易的事：

* 指令cache misses会暂停指令的传递

* 跳转的方向或者地址错误会导致流水线强制清洗

* 现代前端设计中，解析跳转的目标需要访问分支预测器和BTB

在这篇paper中，我们提出了一种新的scalable 前端设计，将分支预测器和BTB与Icache进行解耦，使各自能够发挥最大的性能。我们称之为**Fetch Target Buffer**。

## A Scalable Front-End Architecture

![](https://pic1.zhimg.com/80/v2-6380ef84d9bf72541b79196ed176ee00_720w.jpg)

首先将I-cache与分支预测器进行解耦，在前端的关键路径中消除这个巨大且慢的memory。为了提供被解耦的前端，我们使用了一个**Fetch Target Queue**，用于弥合分支预测器和指令cache之间的差距。每个周期，分支预测器都会产生一个fetch target block预测并且存入FTQ，最终被指令cache使用。因此，FTQ和I-cache能够在对方由于miss而stall时继续工作。

除此以外，流水线化I-cache使在不影响前端关键路径长度的情况下扩大cache size或者associativity。

为了保证不错的吞吐量和可扩展性，分支预测器和BTB都要尽可能小，但是为了获取不错的分支预测能力，又需要较大的分支预测器。因此，我们引入了**Fetch Target Buffer**，构造出一种multi-level的分支预测架构。其用于返回每个周期它所访问的动态指令流的信息。它通过预测地址和fetch block的大小来完成这个目的。每个周期，FTB产生一个下一个fetch block的开始地址，fetch block的结束地址和被预测将在下一个周期被使用的Target地址。

## Fetch Prediction Architectures

### Branch Target Buffers

Yeh and Patt提出使用**Basic Block Target Buffer**。BBTB使用basic block的开始地址来索引，每个entry包含了一个tag，type information，basic block的taken Target地址和basic block的fall-through地址。如果basic block结尾的branch被预测为taken，那么taken地址将会被用于下一个周期的fetch，相反，fall-through地址将被用于fetch。如果发生了BBTB miss，current fetch address加上fixed offset将被用于下一个周期的fetch。

## Fetch Target Buffer

我们的架构模型在BBTB的基础上有两个改变。首先，我们在FTB中不存储fall-through的basic block或者很少发生taken的basic block，这些basic block将会浪费BBTB的entry并且减小fetch block的大小。第二，我们在FTB中不保存full fall-through address，相反，我们只存储被预先计算的fall-through地址的低位以及用于计算剩余fall-through地址的进位。

FTB table使用一个fetch Target block的开始地址来访问。FTB中的每一个entry包含了一个tag，taken address，部分的fall-through地址，fall-through进位，跳转类型，oversize bit和有条件跳转预测信息。

![](https://pic3.zhimg.com/80/v2-bbf8d5dd8ce5a527e0fcf9553ae7ad5a_720w.jpg)

如果进位标志未设置，完整的fall-through地址计算方式为N比特的current fetch地址连接上N比特存储在FTB entry中的fall-through地址。如果设置了进位标志，地址计算方式为N比特存储在FTB entry中的fall-through地址加上1，然后连接上N比特的current fetch地址。

N的大小表示了FTB中的fetch block的大小，如果fall-through超过了fetch block开始地址偏移$2^N$条指令，那么fetch block将被分成多个大小为$2^N$的chunk，并且只有最后一个chunk将被插入到FTB。

oversize bit表示fetch block是否跨越一个cache block。

为了进行分支历史的恢复，一个小的**Speculative History Queue**用于保存分支的推测历史。当进行分支预测时，被更新的local或者global历史将会被插入到SHQ中。当预测结果产生时，SHQ会和L1 FTB并行搜索，如果SHQ中检测到一个更新的历史，它会比L1 FTB中的历史更高优先级。只有当历史改变时，才会在SHQ中分配一个entry。当一个fetch block结尾的分支退休时，它的speculative history将会被写入到FTB。当一个错误的预测被检测到时，SHQ中这个错误分支的指针和被分配的entry会被释放。

### Functionality of the 2-Level FTB

#### L1 FTB Miss and L2 FTB Hit

为了加速搜索，L1 FTB和L2 FTB进行并行访问。在L1 FTB发生miss时，预测器只有Target fetch block的地址却没有size，为了利用该地址，预测器将预先设定的fixed length注入到从未命中fetch block开始的fall-through fetch block。当L2 FTB entry返回时，与推测生成的fetch block进行比较。如果更小，那么L1 FTB在fetch block进行一个pipeline squash。

#### L1 FTB Miss and L2 FTB Miss

L1 FTB进入持续将连续的fetch block注入machine直到错误预测被检测到的状态。当misprediction被检测到，L1 FTB将会纠正关于这个新的fetch block的信息，然后继续进行正常的操作。

#### Branch Misprediction Recovery

FTB entry会用纠正的block information进行更新，SHQ中错误推测的entry会被释放，错误推测的分支后的流水线会被flush。

参考资料：Reinman G, Austin T, Calder B. A scalable front-end architecture for fast instruction delivery[J]. ACM SIGARCH Computer Architecture News, 1999, 27(2): 234-245.
