---
title: Computer Architecture论文阅读笔记（1）
date: 2022-07-08 16:00:09
tags:
- computer architecture
categories:
- article reading note
---
**Post-Silicon CPU Adaptation Made Practical Using Machine Learning**，来自于Intel Labs，ISCA2019，Session 1A: Using AI to Improve Architecture

**Abstract**

这篇论文基于*Intel SkyLake*提出了一种自适应的CPU，能够关闭部署循环并且提供一种定制*post-silicon*的机制。这种CPU提供了一种*predictive cluster gating*，能够通过*clock-gating*未使用的资源来动态地设置集群架构的发射宽度。在SPEC2017上，该CPU与不可自适应的CPU相比能够提高31.4%的PPW，并且在SLA violation方面降低了2个数量级。

<!-- more -->

**Introduction**

模型使用机器学习来从示例数据中离线提取复杂的预测模式并且通过在硬件中部署生成的预测器来达到运行过程中调整配置的效果。

![](https://pic2.zhimg.com/v2-cf4db06c0eda5785c4be8fb71fdc5dbd_r.jpg)

telemetry system能偶捕获架构中的一些事件计数器，这些计数器定期进行快照并发送到微控制器中，在该微控制器中运行着ML模型，能够根据获取的telemetry数据来生成未来的最佳集群配置。

机器学习模型运行在两个模式中：1. *training*，根据可用数据调整模型参数；2. *inference*，训练完的模型在新数据上生成预测结果。在这项工作中，开发ML自适应模型通过：1. 选择合适的telemetry数据流；2. 记录不同工作负载的训练数据集；3. 选择和配置ML模型；4. 运行一种已知的训练算法



**A PREDICTIVE CLUSTER GATING ARCHITECTURE**

自适应CPU模型具有两种模式：提供8-wide execution的高性能模式和保持一个集群活跃的低功耗模式。

为了从高性能模式切换到低功耗模式，调度器首先停止向集群2发送指令，然后一个定制的微指令将register states从集群2复制到集群1，最终集群2 clock gated。而从低功耗模式返回到高性能模式，我们仅仅需要ungate集群2并且更新调度器来向两个集群传输指令（产生的开销可以忽略不计）。



![](https://pic3.zhimg.com/v2-f17a281e89aa346c6c8641367b98fcfa_r.jpg)



**DATASETS**

当工作负载运行时速，我们将其部分指令流记录在trace中，以便在cycle-accurate simulator中回放。该模拟器输出模拟的telemetry数据将被保存为数据集。

为了生成数据，我们在高性能模式和低功耗模式下都进行模拟，每10k指令对IPC和telemetry值进行一次快照。

值得注意的是，ground truth label是针对未来间隔t+2而不是当前间隔t，允许一个完整的间隔在计数器被发送到微控制器后进行预测计算。

![](https://pic2.zhimg.com/v2-0d9363f4b418569c320e33957704d87d_r.jpg)

**METRICS**

在论文中定义了**RSV（Rate of SLA Violations)**，使用该指标的原因有两点：1. 直接反映了客户的目标，即在一个时间窗口内最大限度地减少由于误报门控决策而导致的可感知的性能下降；2. 捕获了工作负载阶段中系统性错误预测的影响。

**METHODS**

使用K-fold cross validation来表征可能的模型行为在工作负载上的分布



**MACHINE LEARNING INFERENCE IN FIRMWARE**

* Multi-Layer Perceptrons
* Random Forests

在树遍历期间，将架构计数器与每个树结点处的阈值进行比较，将遍历指向左节点或者右节点。遍历将一直递归知道到达一个叶子节点，该节点包含一个预测

* Logistic Regression



**EVALUATION**

该工作使用的计数器为： Branch Mispredictions, Instruction Cache Misses, Data Cache Misses, L2 Cache Misses, IPC, I-TLB Misses, D-TLB Misses,  Stall Count。

Dubach等人提出了一个用于ML适应模型的通用框架来优化多值的架构参数。该方法将计数器数据编码为时间窗口内的直方图，并且通过使用在一定范围内的参数值上采样的IPC和telemetry来进行训练，然后拟合softmax回归来预测性能最高的配置，将这称为**Softmax Regression on Counter Histograms (SRCH)**。

![](https://pic3.zhimg.com/v2-f4c34f8bc179cac85055a2d2cff37186_r.jpg)



**参考文献：**

S. J. Tarsa et al., "Post-Silicon CPU Adaptation Made Practical Using Machine Learning," 2019 ACM/IEEE 46th Annual International Symposium on Computer Architecture (ISCA), 2019, pp. 14-26.
