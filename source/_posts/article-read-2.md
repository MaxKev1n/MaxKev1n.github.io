---
title: Computer Architecture论文阅读笔记（2）
date: 2022-07-08 16:04:55
tags:
- computer architecture
categories:
- article reading note
---
**Bit-level Perceptron Prediction for Indirect Branches**，来自于Barcelona Supercomputing Center and Texas A&M University，ISCA2019，Session 1A: Using AI to Improve Architecture 

**ABSTRACT**

由于在执行之前无法确定indirect branch的目标地址，因此高性能处理器依赖于高度准确的indirect branch预测技术来减少control hazards。这篇paper提出了一种在位级别预测目标地址的全新的indirect branch预测方案。该方案根据分支历史的相关性来预测各个branch目标地址的比特。

<!-- more -->

**INTRODUCTION**

这篇paper介绍了一种新的indirect branch预测算法，**Bit-Level Perceptron-Based Indirect Branch Predictor (BLBP)**。BLBP通过分支历史和基于感知机的机器学习来预测各个目标值比特。处理器通过访问一个专门的类BTB的结构在所有可观测到的分支目标中在位级别最匹配的目标。

![image-20220409011516114](https://pic3.zhimg.com/v2-2691a416725931e4cd323b9c01a42dfa_r.jpg)



**RELATEDWORK**

* **Indirect Branch Target Prediction Via Software**：基于软件的策略并非没有硬件成本，通过软件进行indirect branch target prediction的许多方案都需要增加硬件结构或者明确的ISA支持
* **Hardware Indirect Branch Prediction**：因为indirect branch往往是*polymorphic*，或者通向不同的地址，BTB's last-used prediction策略经常是不够的

* **Perceptron-based Branch Prediction**



**Perceptrons For Each Bit of the Target**

BLBP训练感知机的方式与hashed perceptron相同，但并不是为每个目标训练单独的权重，而是训练一个长度为K的用于预测目标地址的每一位的权重向量。

**Predicting with Perceptrons**

首先，预测器使用历史来索引权重表并且提取4个权重，每一个权重对应一个目标位。权重在-7和7之间，假设每一个权重最初等于3。然后预测器计算权重与第一个目标位的点积，相同的4个权重将被用于计算第二个目标的点积。接着，经过比较点积，拥有最大点积的target将被作为预测结果。

当遇到错误预测时，更新预测的准则如下：

对于用于预测真实地址的位*i*的每一个权重

1. 如果*i*是0，则减少权重
2. 如果*i*是1，则增加权重

![image-20220409012612414](https://pic2.zhimg.com/80/v2-3e9246bcae00f42a1149897613ac7d0d_720w.jpg)

**Optimization Techniques**

* **Local History**：预测给定位的第一个权重向量用本地历史的哈希索引
* **History Intervals**：剩余的权重向量根据受*multiperspective perceptron prediction*和*strided sampling hashed perceptron prediction*的*interval capability*启发的历史区间，使用全局历史的哈希进行索引
* **Transfer Functions**：正如*multiperspective perceptron prediction*一样，我们发现在求和之前对权重一个用非线性的transfer function能够提高准确性
* **Adaptive Threshold Training**：与 O-GEHL 和之前的感知器预测器一样，我们使用 Seznec 的自适应阈值训练算法来调整训练阈值，使正确预测的训练实例数大致等于错误预测的数量

* **Selective Bit Training**：如果一个位的值在一组可能的target上不同，则仅在该位上进行预测和训练。
* **BTB Compression**：许多内存区域地址的高位相同，因此我们可以用大约一半的位数来表示64位target。BTB进存储分支PC的一部分tag，权衡了标签意外匹配的低概率和存储部分tag的优势。



**Conclusion**

本文介绍了Bit-Level Perceptron-Based Indirect Branch Predictor或者BLBP。BLBP使用基于感知机的学习来预测indirect branch target的较低位。从IBTB中获取一些已知的target address作为比较，输出与预测位最接近的目标地址作为 BLBP 的预测。本文证明了BLBP的性能优于the state-of-the-art。使用一组包含significant indirect branch的88个benchmark，结果显示BLBP将ITTAGE的预测性能提高了5%，将MPKI从0.193减少到了0.183。

![image-20220409015424757](https://pic1.zhimg.com/80/v2-09c89a9c7e9d53d6677c86f68ad54488_720w.jpg)



**参考文献：**

E. Garza, S. Mirbagher-Ajorpaz, T. A. Khan and D. A. Jiménez, "Bit-level Perceptron Prediction for Indirect Branches," 2019 ACM/IEEE 46th Annual International Symposium on Computer Architecture (ISCA), 2019, pp. 27-38.