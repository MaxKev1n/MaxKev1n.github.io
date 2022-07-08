---
title: Computer Architecture论文阅读笔记（3）
date: 2022-07-08 16:11:20
tags:
- computer architecture
categories:
- article reading note
---
**Division of Labor: A More Effective Approach to Prefetching**，来自Dept. of Electrical and Computer Engineering University of Rochester，ISCA2018，Session 2A: Prefetching

**Abstract**
在这篇paper中，使用一些专门用于一些简单模式的组件，如canonical strided access。具有这些组件的预测器具有更好的性能，在有限的预取范围内实现了更高的精度。

**INTRODUCTION**

一般来说，具有单一target模式的monolithic预取器不太可能在保持高精度的同时具有广泛的预取范围。我们认为在实践中最好在composite预取器中设计多个专门针对自己的重点预取target的组件。通过这种分工，范围和准确性能够进行分离。范围是通过更好的组合实现的，准确性是通过改进组件实现的。我们选择了更集中的组件，每一个组件仅处理有限的情况，当时这样做的效率和准确性都很高。当这些组件共同使用时，它们能够互相补充并形成一个composite预取器，该预取器的范围主要是组件的范围总和，并且其准确性能够不受范围扩大的影响。

<!-- more -->

**BUILDING COMPOSITE PREFETCHERS**

第一个指标用于衡量预取器尝试覆盖missing stream的数量。我们仅仅关心预取了哪些地址，而完全忽略预取是否有效。我们将这个指标称为prefetching scope，衡量预取器在某个时间点尝试的footprint比例。由于并非每一行cache line都同等重要，因此，权重因子$W_i$（对于cache line $A_i$）是地址$A_i$的为命中次数（在特定的观察窗口内）。只要预取器尝试预取cache line（在观察窗口内），该line就被视为已被覆盖，而不需要考虑预取的频率或者效用。

第二个指标，effective accuracy，衡量每一个已发射的预取的有效程度。我们使用被避免的miss数量（由于使用预取器）除以发出的预取数量来定义它。

![](https://pic4.zhimg.com/v2-835972cdf99bed1f30bba0ee001e3483_r.jpg)

不幸的是，以降低准确性为代价来扩大范围将不会带来显著的最终性能优势，同时肯定会增加预取的成本

在composite预取器中，可以通过查找专门的预取器组件来改进范围，这些组件针对的是现有组件尚未涵盖的预取目标。

![](https://pic1.zhimg.com/80/v2-960051baf0d7827cad17cca6993d76c8_720w.jpg)

这种composite预取器能够带来一些好处：

1. **Efficiency**：通过专门化pattern，每一个组件能够最小化存储efficiency。通过适合的协调，每个组件可以忽略已知与其他组件相关联的地址，能够最大限度地减少预取器识别错误pattern的机会。
2. **Clarity**：不同的访问模式具有不同的预测成功概率。给定不同的成功概率，也更容易确定合适的预取目的地。
3. **Flexibility/configurability**：不同的应用会有不同的可能性去展示一定的pattern。根据反馈，程序能够调整不同预取器组件的参数，关闭特定的组件，甚至带来一些ad-hoc预取逻辑。



**EXAMPLE IMPLEMENTATIONS**

**Targeting Strided Streams**

* **Identifying loops**：当执行被分解为循环迭代时，canonical strided streams能够相对容易地被检测并且被预取。循环相关的硬件会尝试去识别这些内部循环。这些循环分支会被用于标记相同代码的迭代之间的边界。为了将这些分支过滤出来，我们使用了loop-branch寄存器来跟踪PC和backward branch的目标。当一个新的backward branch和loop-branch寄存器存储的branch相匹配，那么这个循环就被识别出来。这个简单做法并不能覆盖所有的可能。因此，使用一张表来减少识别稳定循环的时间。在evaluation中，使用了一张具有20个项的**Non-Loop PC Table**（**NLPCT**）来存储这些分支的PC，一旦一个分支在这张表中识别到，它将会被loop marker跳过。

![](https://pic4.zhimg.com/v2-cf6947cda7f67e76aacd80ec094f1f8f_r.jpg)

* **Strided stream detection**：基本思想是跟踪每一条访存指令（通过PC），指令最后执行的地址以及两次连续访问中地址之间的增量。当检测到的增量稳定，我们在I-cache中标记这条指令，因此每次发射逻辑发射这条指令时会通知组件，组件也会相应地预取。但是仍然需要进行两处修改。第一，由于不同的访存指令访问相同的流是普遍的，我们只会在指令触发初始的cache miss（在L1中）才会跟踪指令。这可以通过将访存指令标记为I-cache中的四种状态之一来实现，state0(unknown)，state1(observation)，state2(strided)，state3(non-strided)。第二个修改时消除不同call sites的歧义，这在面向对象编程中十分常见。一种简单的修复是将PC和RAS的顶部项进行异或操作。
* **Prefetching**： 给定循环硬件，我们可以很好地估计每次迭代的执行时间。如果我们跟踪平均内存访问时间，我们可以控制预取距离。



**Targeting Pointer Chains**

第二种访问模式使用了指针，后一次访问的地址依赖前一次访问的结果，这种模式的一个挑战是如何及时地预取。有两种指针访问模式使用相对简易的有限状态机来实现及时地预取

* **Array of pointers**：后一次访问的地址是从一个strided access stream（加上一个恒定的偏移量）获取的值。为了找出特定的strided memory instruction $i$的相关loads，我们在译码阶段使用了一个简单的传播电路。首先与所有逻辑寄存器对应的一个位向量被清零，然后对应于指令$i$的目标寄存器的单个位将被设置。从此，如果一条指令的一个源寄存器在位向量上被设置，那么它的目的寄存器对应的位也将被设置，否则，目的寄存器将被清零。这一过程将会持续到指令$i$再次被遇到。如果一条指令$j$与指令$i$具有一个稳定的增量，那么指令$i$将会在**stride identifier table (SIT)**中被标记，同时记录对应的增量。在稳定状态下，如果指令$i$被执行，该指令将会被发送到组件中并加上增量后发送预取。

![](https://pic4.zhimg.com/80/v2-8731d2b75602f5dbf8be384ef67f4f43_720w.jpg)

* **Pointer chains**：第二种模式是更经典的指令链模式。如果内存指令 i 的地址寄存器依赖于它自己的目标寄存器（来自上一次迭代），那么它就形成了指针链模式。除了增加一个delta（$A_{n+1}=A_n+\bigtriangleup$），我们还需要访问内存（$A_{n+1}=M[A_n+\bigtriangleup]$）。这种模式下的FSM，只会在以前的预取返回结果后发射下一次预取。对于指令链，当我们在错误的track时，FSM会一直在错误的track上执行预取并生成污染。一种解决方案是在 SIT 中保留一个预取地址，并将其与来自相应存储器指令的即将迭代的实际地址进行比较。 如果在超时期间（例如，在 m 次迭代之后）没有找到匹配项，则可以重置内存指令的状态以再次测试指针链模式。

![](https://pic2.zhimg.com/80/v2-0aea44227d36a87379fe270bf190ea15_720w.jpg)



**Targeting High Spatial Locality Streams**

为了跟踪区域中的空间局部性，我们使用了一个**Region Monitor**(**RM**)，包含多个项，每一个项都跟踪区域中的一个cache line。对于每一次访问cache，如果区域在RM中，那么相关的位将被设置，反之将会选择一个无效的或者victim项。在我们的设计中，我们尝试将高局部性访问流与指令相关联，这需要另外一个结构，**Instruction Monitor** (**IM**)。当我们开始监视一条候选指令时，就会在 IM 中分配一个新条目。 该条目将一直保留，直到对候选指令做出决定。当一个被监视的指令访问区域$r$，我们找到它的IM entry ID($k$)，前往该区域$r$在RM中对应的项并且设置该项的$k^{th}$位。当区域条目被驱逐时，我们将按如下方式更新访问该区域的每条指令。IM 中的每个条目都有两个计数器：**TotalRegions** 和 **DenseRegions**。 前者总是递增的； 后者仅在被驱逐区域密集的情况下增加。当*TotalRegions*达到阈值时对指令做出决定。如果一条指令以高概率访问一个密集区域，这条指令将被标记。当这种指令在未来被执行时，组件将会触发区域预取。

![](https://pic3.zhimg.com/v2-7a09414e7041982a9bbc7a5c44dafc0a_r.jpg)



**Coordinator Design**

* 我们可以测量每一个组件的有效精度并为每个模式选择性能最佳的组件
* 我们能够根据静态指令在经验上和概率上建立合理的分工

![image-20220410153333198](https://pic3.zhimg.com/80/v2-f0d8ba16fcd9cd0b1bdf14d9c602615a_720w.jpg)



**Existing Prefetchers as Components**

如果我们盲目地让所有组件尝试，那么一些预取器获得正确预取的可能性会更高，但可能会有更高程度的污染来抵消甚至抵消收益。

首先，我们希望识别一个对特定访问模式最优的组件来处理它。其次，一旦识别出一个组件，就会从其他组件中过滤掉相关的访问，以最大程度地减少错误的预取。当有不止一个组件时，我们以循环方式将访问分配给每个组件，预取的行将由发出预取的组件的标识进行标记。当一个访问需求命中预取行时，我们将使用引入该行的组件来处理指令。



**EXPERIMENTAL ANALYSIS**

**Overall Effect of the Example Implementation**

本文的主要猜想是通过适当的预取组件之间的分工，我们可以创造比传统的monolithic prefetcher更加有效和高效的composite prefetcher。

* **Effectiveness**：TPC 的几何平均加速为 1.41，而monolithic设计为 1.21 到 1.33。换句话说，TPC 比最接近的竞争对手快 6%。 其次，TPC 是广泛有效的。 在 21 个基准测试中，它是 11 个中性能最好的预取器，在其余基准测试中，它的性能与最佳预取器相差不到 5%。

![image-20220410155452728](https://pic4.zhimg.com/80/v2-e73db4b7c3dc186a4bfc5bbb7503fc93_720w.jpg)

* **Efficiency**：平均而言，TPC 下的内存流量开销为 6%，是经过测试的硬件预取器中最少的。

![](https://pic1.zhimg.com/v2-9206cdda8a65dae6cfe4f8789240a644_r.jpg)

* **Different workloads**：TPC 始终优于其竞争对手，尽管效果不同。 取所有 68 个工作负载的几何平均结果，TPC 实现了 1.39 的加速，而其他七个预取器为 1.22-1.31。

![](https://pic3.zhimg.com/v2-38bab7dc491e0b94463166447147a0ea_r.jpg)



**参考资料**：

S. Kondguli and M. Huang, "Division of Labor: A More Effective Approach to Prefetching," 2018 ACM/IEEE 45th Annual International Symposium on Computer Architecture (ISCA), 2018, pp. 83-95, doi: 10.1109/ISCA.2018.00018.

