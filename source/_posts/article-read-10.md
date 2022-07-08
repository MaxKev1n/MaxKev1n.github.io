---
title: Computer Architecture论文阅读笔记（9）
date: 2022-07-08 16:23:11
tags:
- computer architecture
categories:
- article reading note
---
# Software Trace Cache

## Abstract

**Software Trace Cache** 算法以链的方式组织basic block，试图使顺序执行的basic block驻留在连续的内存位置，然后将basic block chain映射到内存中来最小化冲突miss。

<!-- more -->

## introduction

影响fetch性能的主要有三个因素：

* memory latency：从内存中读取指令的时间

* fetchwidth：每个周期能够获得的指令数

* branch prediction accuracy：错误执行路径中的指令数

使用编译器优化现有的应用有两个好处：

* 没有任何的硬件消耗，不需要增加额外的transistor和功耗

* 为现有的架构提供性能改良

## The STC Algorithm

我们在算法的单次通过中构建所有的basic block trace，无需任何用户干预来确定阈值。我们使用一种自动的处理来选择我们的basic block trace的开始点。最终，我们映射整个basic block trace到**Conflict Free Area**中。

### Seed Selection

在我们将basic block set组织进traces前，需要为这些trace选择一个seeds或者开始点。我们选择了所有子路径entry points作为seeds。我们维护了一个以basic block weight为排序顺序的列表：从最常被执行的seed到最少被执行的。

### Trace Construction

从选定的种子中，我们继续使用贪心算法，该算法跟随基本块中最可能的路径，记录所跟随的路径作为所需的轨迹。当一个基本块的所有目标都已被访问或遇到主程序的子程序返回时，跟踪结束。

loop的处理方式相同。算法跟随loop body中最有可能的路径直到backward branch的边沿被找到。在STC算法中，loop并不会被展开。

![](https://pic1.zhimg.com/80/v2-c390de76099325078b18bc26ec1ef100_720w.jpg)

## Trace Mapping

我们按照创建的顺序映射生成的跟踪：从最常执行的到执行最少的。在这种方式下，我们平等地将常用的traces映射到其他的traces边上，减少他们之间的冲突。同时，我们将traces分成instruction cache-sized chunks并且在出了第一个block外的所有block的开头留下一个空的空间。

所有的code gaps映射到指令cache中的相同位置，因此其他的code不会被作为最常用的trace被映射到相同的位置，这为这些traces创造出一个CFA。

为了获得一个合适的CFA size，我们首先将所有常用的trace取出。然后，我们我们将它收集的总执行时间的百分比与它所需的指令缓存的百分比进行比较。如果执行的百分比高于其所需空间，那么我们将trace放入CFA。我们接下来加上下一个trace，并且计算他们共同的执行百分比以及它们需要的缓存百分比。只要执行的比例比所需的指令cache比例大，就继续讲traces放入CFA中。

![](https://pic3.zhimg.com/80/v2-6cfe604b04aa131873496f7559023366_720w.jpg)



参考资料：Ramirez A, Larriba-Pey J L, Valero M. Software trace cache[J]. IEEE Transactions on Computers, 2005, 54(1): 22-35.
