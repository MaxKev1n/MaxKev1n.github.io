---
title: Computer Architecture论文阅读笔记（9）
date: 2022-07-08 16:21:47
tags:
- computer architecture
categories:
- article reading note
---
## Abstract

这篇文章中，我们提出了一个基于长连续指令流的执行的fetch架构，达到code layout优化的最大效果。fetching instruction stream有效地利用了layout优化后的代码的特殊性质，来获得更好的fetch性能。

<!-- more -->

## Introduction

一个指令流由它的开始指令地址和流的长度来分辨。与traces不同，指令流并不要求branch的行为被包含在流中，因为这在定义中暗示了：所有的intermediate的branch不会taken，而terminnating的branch总会被taken。

![](https://pic4.zhimg.com/80/v2-b07ab9507978a5714db265a6e2a426b7_720w.jpg)

假如指令流中发现一个分支预测错误，就会告知前端engine，fetch将会被重定向，并且从错误预测的点继续运行。错误分支预测前的指令将会被执行并且正常地退休。前端在branch后修复，并且fetch被重定向到正确的branch Target。

为了避免一些错误，我们讲一个部分的指令流定义为一个流，其开始语一个错误分支预测的Target，一直到下一个taken branch。这使得前端engine能维持stream semantics，即使在一个错误的分支预测事件中。

## The stream fetch architecture

![](https://pic4.zhimg.com/80/v2-1e5b131210fc77301affb09819dcb217_720w.jpg)

next stream predictor为fetch engine提供了stream level sequencing。给定的现在的stream开始地址，提供了现在的stream长度以及下一个stream的开始地址。被预测的下一个stream地址作为在下一个周期的fetch地址。现在的stream地址和长度被存储在**fetch Target queue（FTQ）** 中，代表了对一个完整指令流的fetch request。

指令cache被FTQ中存储的fetch request驱动。steam开始地址用于访问指令cache，提供了一个或者多个连续的cache line。如果cache line包含了一个完整的stream，那么FTQ将会被移动到下一个request，否则，fetch request被更新，来反映剩余的流中将被fetch的指令。

## Complexity issues

我们的stream fetch架构，有一个单独的指令路径，单独的分支预测器，单独的存储cache，不需要大量的控制逻辑来协调不同的组件。

## Stream prediction

next stream predictor功能包括分支预测器和目的地址预测器，代替conditional分支预测器和传统fetch engine的BTB/FTB。所有从stream开始地址起始的intermediate branch都被预测为not taken，而terminating branch被预测为taken。所有not taken的分支的目标地址都为下一个连续的指令，terminating branch的目标地址为下一个流的开始地址。

![](https://pic2.zhimg.com/80/v2-e3ba9c041081870a0b5348e2e9eaaa91_720w.jpg)

每一个table entry包含了一个指令流的信息：开始地址，长度，terminating branch类型，下一个流的地址，一个2比特的saturating 计数器用于替换策略。

第一个table使用现在的fetch address来索引，第二个table使用现在的fetch address和之前的流的开始地址(之前的fetch address)的hash来索引。

预测器维护了两个不同路径 历史寄存器：一个*lookup register*使用推测的信息like更新，一个*update register*在当流结束的commit time，使用正确的path信息来更新。在错误的预测中，non-speculative register中的内容被复制到speculative register，恢复正确的历史状态。

预测器使用现在的fetch address计算hash，并将其作为prediction table的索引。如果两个table都命中率，我们选择path correlated table中的数据。如果仅命中一个，我们选择命中的数据。如果都未命中，我们诉诸顺序提取，直到预测器再次命中，或者检测到分支预测错误。

path correlation和hysteresis计数器的使用是为了允许stream预测器能够在prediction table中保持重叠的stream。这使得stream能够保持尽可能的久，不需要被迫将它们切短来避免多个fetch block的重叠。

stream在第一次出现时被插入到两个表中，当其再次出现时，它的信息只在尚未被替换的表中更新。那些不需要patch correlation for accurate prediction的stream将会在第二个表中被替换，但是第一个表仍然能够预测他们。如果预测错误，仅存在于第一个表中的stream将会升级到第二个表。因此，不需要path correlation的流将永远不会升级到第二个表，从而避免了别名。

## Fetch target queue

我们使用FTQ的目的不是为了实际提高性能，而是为了使分支预测器能够与指令cache运行在不同的速率下。一个单独的FTQ entry花费多个周期来从指令cache中fetch，这使得分支预测器能够比指令cache运行得更快，并且在FTQ满时stall。

一个流平均含有超过16条指令，意味着平均存储在FTQ中的fetch request过大而难以在一个周期内被fetch。我们实现了一种fetch request update机制而不是将fetch request分割为更小的。

![](https://pic3.zhimg.com/80/v2-edd4daa7df5ff91d262e97ae50f2a292_720w.jpg)

每个fetch request包含了stream的开始地址和长度。开始地址用于访问指令cache并且fetch多个连续的cache line。取决于被fetch的cache line的数量以及cache line的宽度，一些水stream的指令将会被fetch。fetch request将会使用从指令cache中获取的指令数来进行更新。流的开始地址会被前移，长度会被正确地减去。如果流的长度变成0，fetch request会满足，而FTQ也会移动到下一个request。



参考资料：Ramirez, Alex, et al. "Fetching instruction streams." *35th Annual IEEE/ACM International Symposium on Microarchitecture, 2002.(MICRO-35). Proceedings.*. IEEE, 2002.
