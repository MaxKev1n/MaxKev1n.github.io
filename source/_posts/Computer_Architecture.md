---
title: Computer Architecture
date: 2022-07-08 15:35:50
tags:
- computer science
categories:
- other study
---
**现代计算机系统抽象**

1. Application
2. Algorithm
3. Programming language
4. Operating System/ Virtual Machine
5. Instruction Set Architecture
6. Register-Transfer Level
7. Gates
8. Circuits
9. Devices
10. Physics

<!-- more -->

* Instruction Level Parallelism
  * Superscalar
  * Very Long Instruction Word(VLIW)
* Long Pipelines
* Advanced Memory and Caches
* Data Level Parallelism
  * Vector
  * GPU
* Thread Level Parallelism
  * Multithreading
  * Multicore
  * Multiprocessor
  * Manycore



"Architecture"/Instruction Set Architecture:

* Programmer visible state(Memory & Register)
* Operations (Instructions and how they work)
* Exeutions Semantics (interrupts)
* Input/Ouput
* Data Types/Sizes

Microarchitecture/Organization:

* Tradeoffs on how to implement ISA for some metrics (Speed, Energy, Cost)

例如：Intel的X86指令集就是Architecture，而AMD和Intel对于X86指令集各自的实现方式，不同版本的芯片则是Microarchitecture



### Memory Technology

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/1.png)

这是一个简单的Register File，寄存器的宽度都为32bits，同时它们的总线也为32bits。通过2位的Read Address信号读取四个寄存器值，而Write Address信号通过下方的译码器对相应的寄存器赋写使能。

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/2.png)

Address通过row 译码器送入到word lines，包括read word lines和write word lines，最终进入到每一个单独的存储单元(single storage element)，即bit cell

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/3.png)

 与Register file相比，SRAM通常被制作为单端口的内存，而Register file通过被制作为多端口的内存。Register File具有更高的速度以及更多的端口，而SRAM具有更高的密度

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/4.png)

DRAM不使用CMOS Logic技术，它的每一个cell仅包含一个电容器通过晶体管与bit line相连接。DRAM的优点在于很容易集成大量存储单元在相同面积的区域内，但是DRAM具有一个问题，它的存储单元会持续地掉电，因此需要刷新DRAM来为存储单元上电

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/5.png)

### Cache

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/6.png)

* 容量: Register << SRAM << DRAM
* 延迟: Register << SRAM << DRAM
* 带宽: on-chip >> off-chip

使用cache的重要原因:**Temporal Localiy**和**Spatial Locality**



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/7.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/8.png)

Cache映射策略:

* 全相联
* 组相联
* 直接映射



如何寻找cache中的block?

* 通过index和offset进行匹配,然后检查tag
* tag检查仅仅包括higher order bits
* 对于组相联cache,cache对所有符合匹配的block并行进行tag检查



如何进行块替换?

* 直接映射的cache没有其他选择
* 随机
* Least Recently Used(LRU)
  * LRU cache状态在每次访问时都应该被更新
  * 4-8路的cache通常使用Pseudo-LRU binary tree

* FIFO aka round-Robin
  * 在highly associativeg caches中使用
* Not Most Recently Used(NMRU)
  * 最近被使用过的block应用FIFO策略时会产生异常



写策略:

* Cache Hit
  * 写直通法:将数据写回cache和memory,通常具有更高的traffic
  * 写回法:只将数据写回cache,当cache中的block被淘汰时才将数据写入memory
* Cache Miss
  * No Write Allocate:只将数据写到主存中(容易经常造成Miss)
  * Write Allocate:先将block存入cache,然后写数据
* Write Through & No Write Allocate
* Write Back & Write Allocate
* 这两种写策略都可以使用写缓冲区来允许缓存在数据放入缓冲区后立即处理，而不是等待一个完整的延迟后再将数据写入内存(Both write strategies can use a write buffer to allow the cache to proceed as soon as the data are placed in the buffer rather than wait for full latency to write the date into memoty)



#### Cache performance

三种Missing:

* Compulsory: 第一次对一个block进行调用, occure even with infinite cache
* Capacity: cache容量太小以至于无法存储程序所需的数据

* Conflict: 多个block被映射到同一个位置上导致一部分数据被替换

**Average Access Time = Hit Time + (Miss Rate * Miss Penalty)**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/9.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/10.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/11.png)

**如果cache的大小变成双倍,那么miss rate通常会下降$\sqrt{2}$倍**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/12.png)

**大小为$N$的直接映射cache的miss rate与大小为$N/2$的二路组相联cache大致相等**



1. 更大的block size以减少miss rate，但是增加了capacity或者conflict misses
2. 更大的cache以减少miss rate，增加了hit time和更高的cost、power
3. 更高的关联性以减少miss rate，减少了conflict misses，但是增加了能耗
4. 多级cache以减少miss penalty
5. 给予读未命中相比于写更高的优先级以减少miss penalty(Giving priority to read miss over writes to reduce miss penalty)，这个选择会对能耗有微小的影响
6. 在cache索引的过程中避免地址翻译以减少hit time



> 设计师可以通过减少行的数量来增加block size(保持整个cache大小不变)，但是这将会增加miss rate，尤其是在较小的L1 cache中

> 测试指出way prediction的准确率在二路组相联中能够超过90%，在四路组相联中能够超过80%，I-cache具有比D-cache更高的准确率。

---

### SuperScalar

Data-dependence: Read-after-Write (RAW) hazard

Anti-dependence: Write-after-Read (WAR) hazard

output-dependence: Write-after-Write (WAW) hazard



> Superscalar处理器通过并行执行多条指令来确保*CPI < 1 (IPC > 1)*

 

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/13.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/14.png)



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/15.png)

有时候，取指令时会遇到特殊情况，比如从cache的不同block中取指令或者从cache的不同line中取指令，甚至随机地从cache中取指令，因此我们需要cache具有额外的端口

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/16.png)
![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/17.png)

当具有Alignment Constraints时，我们不得不同时取包含了多余指令的两条指令，其中一条不应该进入流水线

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/18.png)

当出现interrupt时，我们应该让两条指令都运行到流水线的末端，并且将前一条指令完成提交，而不是将前一条指令杀死

#### Breaking Decode and Issue Stage

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/19.png)



> **将流水线的Decode Stage分割为Decode 和 Issue两个stage，将会增加分支的penalty**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/20.png)


#### Exception & Interrupt

异步：外部事件

* 输入输出设备服务请求
* 计时器终端
* 硬件错误

同步：内部异常(a.k.a.异常或者trap)

* 非法指令，特权指令
* 算术溢出，FPU异常
* 未对齐的内存访问
* 虚拟内存异常
* 软件异常：系统调用



> 由于异步异常不和特定的指令挂钩，因此难以确定发出中断的时间

当中断发生时，处理器如何进行处理：

* 处理器将正在运行的程序在指令$I_i$处停止，并且完成所有$I_i$之前的指令(up to $I_{i-1}$)
* 将指令$I_i$的PC值保存在一个特定的寄存器中（EPC）
* 处理器关闭中断使能并且将控制权移交值运行在内核模式的指定的中断处理器



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/21.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/22.png)

五级流水线在访存级之后，写回级之前设置一个Commit Point，当发生异常时，异常信息随着指令执行到达Commit Point时发出kill信号，将流水线中正在执行的所有指令杀死并且对指定的异常处理代码进行取指令操作

---

#### Out-Of-Order

*front end*: fetch and decode

*Scoreboard*: 保存已经准备好执行的指令的信息

*Reorder Buffer*: 对乱序执行的指令重排序来使得指令按序提交

*Store Buffer*: 存储内存数据直到Commit Point，来使得内存数据不被过早地写入

*Issue Queue*: 当Issue是乱序时，用于判断发射指令是否安全，更重要的是用于保持依赖，如RAW,WAW,WAR

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/23.png)



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/24.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/25.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/26.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/27.png)

**Architectural Register File (ARF)**: 存储了被提交的体系寄存器的状态

**Scoreboard (SB)**: 跟踪这些在不同流水线中不同值的位置



由于仅需要得知旁路中一个值的最新数据，Scoreboard的Functional Unit将会记录发出的最新指令的位置

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/28.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/29.png)



##### I2O2

**I2O2 Scoreboard**

* 与I4相似，但是我们现在使用它来跟踪写回端口的structural hazards
* 根据流水线的长度，在Data Avail.中设置比特位
* 为了避免WAW hazards，体系结构将会在译码阶段进行stall，因此目前的scoreboard已经足够了

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/30.png)



**sliding commit point**（略）



##### I2O1

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/31.png)

**Physical Register File (PRF)**: 这里面的值并没有被提交到处理器中，如果出现异常或者跳转，那么里面的值有可能被丢弃

**Reorder Buffer (ROB)**: 进入ROB的指令可能是乱序的，但是离开ROB的指令是有序的

**Finish Store Buffer (FSB)**: 当我们有需要存储到内存中的数据，由于处理器难以roll back，我们并不想过早地提交数据，因此我们将数据存储在FSB中，当我们有load操作时，应当对FSB具有相比于cache更高的访问优先级



对于I2O1处理器，我们仅在发生branch或者interrupt时对ARF进行访问，并且将数据转储到PRF中。SB与其之前不同之处在于，它不在跟踪ARF，而是跟踪PRF。当一个指令被发射时，我们将会为其在ROB中分配一个位置，当位于流水线末端时，我们将要进行指令提交时，我们将这条指令从ROB中清除。当数据被提交到memory中时，我们对FSB进行清除

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/32.png)

如果ROB中最老的指令仍在流水线中，我们无法进行任何的提交。speculative通常用于branch，对于speculative段为1的指令，如果我们发现进行了错误的预测，我们可以立即清除它们而不用担心任何数据问题。V字段代表对寄存器进行写操作，当该指令到达流水线的末端时，对Preg字段填写Physical register file entry，即这个值的目的地。

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/33.png)

如果你的设计中允许流水线中同时存在多个load、store操作，那么应当具有一个从FSB到load的旁路



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/34.png)



当出现exception时，PRF中可能存在部分错误的寄存器值，这就需要我们用ARF覆盖PRF来使PRF的状态进行roll back



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/35.png)

上述的几种选择的性能为，*Option1 > Option2 > Option3*

有时候我们可能遇到cache写不命中，导致Commit需要占用多个周期，并且令剩余的指令被迫推迟多个周期。因此，我们可以在Commit后面添加一个额外的stage用于cache写不命中后进行写操作

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/36.png)



##### IO3

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/37.png)

**Issue Queue**: 有序地存放指令，乱序地发射指令。当ARF中的操作结束时，我们会标记IQ中的一些bits，表示准备使用的寄存器已经就绪。由于跟踪指令存活的需求，我们会直到流水线的末端才将指令从IQ中移除。

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/38.png)

当一条指令被提交时，我们会清除该指令对应的IQ行，并且在其余IQ记录中的Src字段搜索与该指令的Dest相等的记录，如果存在与Dest相同的Src，则将该Src的P字段更改为not pending

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/39.png)

*Tomasulo算法（推荐）*



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/40.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/41.png)

*如果第六条指令提前一个周期发射，就会与第2条指令发生Write Back的Structural Hazard；如果提前两个周期发射，就会与第3条指令发生Issue的Structural Hazard*



##### IO2I

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/42.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/43.png)



---

#### Speculations and Branches

* 在I4处理器中，当我们出现错误的分支预测时，我们将会清除Scoreboard
* 在I2O2处理器中，由于是顺序发射的，我们可以在执行跳转时，直接对流水线的全部进行重定向，清除剩余的所有指令，并且重置Scoreboard
* 在I2OI处理器中，我们必须在跳转之后立即清除流水线中的指令来阻止写入PRF。我们可以将指令立刻从ROB中清除，或者直到提交后才清除
* 在IO3处理器中，我们需要在跳转时进行stall来保证处理器不会产生错误的结果
* 在IO2I处理器中，由于我们需要对跳转后的指令进行清除，并且保证之前的指令正确地执行完毕，我们需要对PRF有选择性的roll back。因为我们确定之前的指令已经执行完毕，ARF相对于跳转已经更新完毕，所以我们可以用整个ARF来覆盖PRF来高效地进行roll back



#### Register Renaming

WAW和WAR并不是“true”data dependencies，RAW是“true”data dependencies

WAW在部分处理器的流水线中将会造成写入寄存器的值错误

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/44.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/45.png)



**Register Renaming**: 通过改变硬件中寄存器的命名来减少WAWH和WAR hazards

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/46.png)

通过在IQ和ROB中设置指针来使我们能够在它们内部使用不同的寄存器名称

Tomasulo算法



**free list**: 跟踪我们可以使用的physical register

**rename table(rat)**: 从ARF映射到最新版本的PRF。但我们发射一条指令进入流水线时会进行更新，当我们的指令到达流水线的末端时，我们也会更新pending bits

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/47.png)



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/48.png)

**Areg**: 当指令提交时，告诉我们应当写入的位置

**Ppreg**: 在流水线前端时从Rename table中读出的，告诉我们在更新前寄存器中存储的值



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/49.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/50.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/51.png)

当我们对一个Physical Register进行分配时，我们必须将它从free list中去除。当我们将free list中的physical register用完时，我们必须对流水线进行stall，因为我们无法进行更多的重命名。当一条指令被提交时，我们将这条指令所分配的physical register重新加入free list

我们直到重写physical register所对应的architectural register时，才会将被分配的physical register从free list中移除

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/52.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/53.png)



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/54.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/55.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/56.png)

修改过的ROB将可以存储数据用于代替在PRF中等待提交的值，当指令处于pending状态时，数据将会存放在Reorder Buffer Entry中，当指令被提交时，数据被写入ARF中



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/57.png)

我们在IQ中并不存放值，我们存放在reorder buffer中的identifier



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/58.png)

当值被存放进reorder buffer中，并且指令已经结束但尚未提交，它会指示我们去哪里寻找值。或者，当最新的值已经存放在ARF中，我们应该ARF中寻找。

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/59.png)



#### Memory Disambiguation

这是一种内存系统中的RAW hazard的analog

* 对于store命令，我们可以将其分为两个部分，地址计算和数据写入
* 对于load命令，我们将指令的地址和它之前尚未提交的指令地址进行比较

**Address Speculation**: 我们假设load和store指令的地址不同，并且进行指令操作，当两个地址相等时，我们将剩余的指令全部杀死并且重新进行指令操，但是Address Speculation容易造成巨大的penalty

> 在 *Alpha 21264*中使用了一种被称为**Memory Dependence Prediction**的方法

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/60.png)

* 在store执行时: 将entry标记为valid和speculative，并且保存数据和指令的tag
* 在store提交时: 将speculative bit清除，并且最终将数据移至cache
* 在store终止时: 将valid bit清除

* 当数据同时存在于store buffer和cache中时，我们应当使用Speculative Store Buffer中的数据
* 当在store buffer中存在两条相同的地址时，我们应当按照指令的顺序读取数据



### Branch Prediction

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/61.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/62.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/63.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/64.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/65.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Computer%20Architecture/66.png)

