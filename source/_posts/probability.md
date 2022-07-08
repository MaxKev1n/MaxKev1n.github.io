---
title: probability
date: 2022-07-08 15:49:26
tags:
- computer science
categories:
- learn in NJUST
---
## 事件的概率

* 非负性
* 可列可加性

* 规范性：*P(Ω) = 1*

性质3：设A，B是任意的两个事件，则***P(B-A) = P(B) - P(AB)***

性质5**（加法公式）**：设A，B是两个事件，则***P(A∪B) = P(A) + P(B) - P(AB)***

<!-- more -->
------

### 古典概型

1）试验的样本空间中<u>元素的个数有限</u>

2）试验中每个结果发生的<u>可能性相同</u>

称满足上面两个特定的试验模型为**古典概型**或**等可能概型**

古典概型中事件A概率的计算公式为
$$
P(A)=\frac{A中包含的样本点数}{样本空间样本点总数}=\frac{N(A)}{N(Ω)}
$$

------

### 条件概率

定义1：设A，B是两个事件，且*P(A) > 0*，称
$$
P(B\mid A)=\frac{P(AB)}{P(A)}
$$
为在事件A发生的条件下，事件B发生的**条件概率**

* 非负性
* 规范性：*P(Ω|A) = 1*
* 可列可加性



**乘法公式：*P(AB) =P(A) P(B|A)***       推广到三个事件：***P(ABC) = P(A) P(B|A) P(C|AB)***



* 定义2：设A、B是两个事件，若P(AB) = P(A) P(B)，则称A、B**相互独立**，简称A、B**独立**

* 性质1：设A、B是两个事件，且*P(A) > 0*，若A、B相互独立，则***P(B) = P(B|A)***

* 性质2：若两个事件A、B相互独立，则A与$\overline{B}$，$\overline{A}$与B，$\overline{A}$与$\overline{B}$也相互独立



* 定义3：设$A_1$,$A_2$,...，$A_n$(n$\geq$2)个事件，若其中任意k（2$\leq$k$\leq$n)个事件$A_{i_1}$，$A_{i_2}$,...,$A_{i_k}$，1$\leq$$i_1$$\leq$$i_2$＜...＜$i_k$$\leq$n，总有

$$
P(A_{i_1}A_{i_2}...A_{i_k})=P(A_{i_1})P(A_{i_2})...P(A_{i_k})
$$

则称$A_1$，$A_2$，...，$A_n$**相互独立**

------

### 全概率公式与贝叶斯公式

定理1：设试验E的样本空间为Ω，B为E的事件，$A_1$，$A_2$，...，$A_n$为样本空间Ω的一个划分，且$P($$A_i$) > 0($i$ = 1，2，...，n)，则
$$
P(B)=\sum_{i=1}^nP(A_i)P(B\mid A_i)
$$
称为**全概率公式**



定理2：设试验E的样本空间为Ω，B为E的事件，$A_1$，$A_2$，...，$A_n$为样本空间Ω的一个划分，且P($A_i$)＞0($i$ = 1，2，...，n)，P(B)＞0，则
$$
P(A_i \mid B)=\frac{P(A_i)P(B \mid A_i)}{\sum_{j=1}^nP(A_j)P(B \mid A_j)}， i =1，2，...，n
$$
称为**贝叶斯公式**

------

## 随机变量及其分布

#### （0-1）分布

若随机变量X的可能取值只能是0与1，且其分布律为
$$
P\{X=k\}=P^k(1-p)^{1-k}， k=0，1
$$
则称X服从参数为p的**（0-1）分布**或**两点分布**，也可写成

| X     | 0     | 1    |
| ----- | ----- | ---- |
| $P_k$ | 1 - p | p    |

#### 二项分布

定义2：设随机变量X表示n重伯努利试验中事件A发生的次数，$P(A) = p$，则称X服从参数为n，p的**二项分布**，记作$X\sim B(n,p)$.此时，X的分布律为
$$
P(X=k)=C^k_nP^k(1-p)^{n-k}，k=0，1，2，...，n,
$$
特别地，当$n=1$时，$P\{X=k\}=P^k(1-p)^{1-k}，k=0，1，$此即（0-1）分布

#### 泊松分布

若随机变量X的分布律为
$$
P\{X=k\}=\frac{\lambda _ k}{k!}e^{-\lambda}，k=0，1，2，...，
$$
其中$\lambda$>0是常数，则称X服从参数为$\lambda$的**泊松分布**，记为$X\sim \pi (\lambda)$

------

### 随机变量的分布函数

定义1：设X是一个随机变量，x是任意实数，函数
$$
F(x)=P\{X\leq x\}
$$
称为X的**分布函数**

性质：

* $0\leq F(x)\leq 1$，且$F(-\infty)=lim_{x \to -\infty}F(x)=0,F(+\infty)=lim_{x \to +\infty}F(x)=1$
* $F(x)$是单调不减的函数，即对任意实数$x_1<x_2$，$F(x_1)\leq F(x_2)$
* $F(x+0)=F(x)$，$F(x)$是右连续的函数

------

### 连续性随机变量及其概率密度

定义1：设随机变量X的分布函数为$F(x)$，若存在非负函数$f(x)$，使对于任意实数x有
$$
F(x)=\int_{-\infty}^{x}f(t)dt
$$
则称X为**连续型随机变量**，其中非负函数$f(x)$称为随机变量X的**概率密度函数**，简称为**概率密度**或**密度函数**

性质：

* 非负性 $f(x)\geq 0$
* 归一性 $\int_{-\infty}^{+\infty}f(x)dx=1$
*  对任意实数$a,b(a<b)，$有$P\{a<X\leq b\}=F(b)-F(a)=\int_a^bf(x)dx$
* 若$f(x)$在点x处连续，则有$F'(x)=f(x)$

#### 均匀分布

若随机变量X的概率密度为
$$
f(x)=\left\{
\begin{aligned}
\frac{1}{b-a}，a<x<b; \\ 0,其他
\end{aligned}
\right.
$$
则称X在区间$(a,b)$内服从**均匀分布**，记作$X\sim U(a,b)$

X的分布函数为
$$
F(x)=\left\{
\begin{aligned}
0，x<a；\\
\frac{x-a}{b-a}，a\leq x<b；\\
1，x\geq b
\end{aligned}
\right.
$$


#### 指数分布

若随机变量X的概率密度为
$$
f(x)=\left\{
\begin{aligned}
\lambda e^{-\lambda x}，x>0；\\
0, 其他，
\end{aligned}
\right.
$$
其中$\lambda >0$为常数，则称X服从参数为$\lambda$的**指数分布**，记作$X\sim Exp(\lambda)$

X的分布函数为
$$
F(x)=\left\{
\begin{aligned}
1-e^{-\lambda x}，x>0；\\
0，其他 \\
\end{aligned}
\right.
$$

#### 正态分布

若随机变量X的概率密度为
$$
f(x)=\frac{1}{\sqrt{2\pi \sigma}}e^{-\frac{(x-\mu)^2}{2\sigma ^2}}，-\infty <x<+\infty
$$
其中，$\mu$，$\sigma$>0为常数，则称X服从参数为$\mu$，$\sigma ^2$的**正态分布**，记作$X\sim N(\mu ,\sigma ^2)$

特别的，$\mu =0$，$\sigma =1$的正态分布$N(0,1)$，称为**标准正态分布**，其概率密度分别记为$\varphi (x)$，$\phi (x)$，即
$$
\varphi (x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}，-\infty <x<+\infty
$$

$$
\phi (x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x} e^{-\frac{t^2}{2}}dt，-\infty <x<+\infty
$$

------

### 随机变量函数的分布

#### 离散型情形

求离散型随机变量函数的分布律，首先找到函数$Y=g(X)$的可能取值，然后求$Y$取这些值的概率即得。计算关于$Y$的某个事件的概率，利用$Y=g(X)$转化成关于$X$的某个事件的概率，然后利用$X$的分布律求出

#### 连续型情形

若随机变量$X$是连续型随机变量，其概率密度为$f(x)$，$Y=g(X)$是$X$的函数，当$Y$也是连续型随机变量时，它的概率密度函数可通过如下两步算的：

1. 计算$Y$的分布函数 $F_Y(y)=P\{ Y \leq y\}=P\{ g(X)\leq y \}=\int_{g(x)\leq y}f(x)dx$
2. 求$Y$的概率密度$f_Y(y)=F'_Y(y)$

这种方法称为“分布函数法”



定理1：设随机变量$X$的概率密度为$f_X(x)$，$y=g(x)$关于$x$处处可导，且是$x$的严格单调增（或严格单调减）函数，则$Y=g(X)$的概率密度为
$$
f_Y(y)=f_X(g^{-1}(y))\mid \frac{d}{dy}g^{-1}(y)\mid
$$

------

## 多为随机变量及其分布

### 二维随机变量的分布

#### 二维随机变量的分布函数

定义1：设$(X,Y)$是二维随机变量，$(x,y)$是任意实数对，即
$$
\{X\leq x，Y\leq y\}=\{X\leq x\}\cap \{Y\leq y\}，
$$
称二元函数
$$
F(x,y)=P\{X\leq x，Y\leq y\}
$$
为二维随机变量$(X,Y)$的**分布函数**，或成为随机变量$X$，$Y$的**联合分布函数**

性质：

* $0\leq F(x,y)\leq 1$，且对任意固定的$y$，$F(-\infty ，-\infty)=\lim_{x\to -\infty y\to -\infty}F(x,y)=0$
* $F(x,y)$是变量$x$或$y$的单调不减函数，即对于任意固定的$y$，当$x_1<x_2$时，对于任意固定的$x$，当$y_1<y_2$时，$F(x_1,y)\leq F(x,y_2)$
* $F(x,y)$关于变量$x$或变量$y$是右连续的，即有$F(x,y)=F(x+0,y)，F(x,y)=F(x,y+0)$
* 对于任意实数$x_1\leq x_2，y_1\leq y_2$，有$F(x_2,y_2)-F(x_2,y_1)-F(x_1,y_2)+F(x_1,y_1)\geq 0$

#### 二维离散型随机变量的分布律

定义2：设二维离散型随机变量$(X,Y)$所有可能取值为$(x_i,y_j)，i，j=1，2，...，$则称
$$
P\{ X=x_i，Y=y_j\}=p_{ij}，i，j=1，2，...
$$
为二维离散型随机变量$(X,Y)$的**分布律**，或称为$X，Y$的**联合分布律**

显然，$p_{ij}\geq 0，i，j=1，2，...$;          $\sum_{i,j}p_{ij}=1$

#### 二维连续型随机变量的概率密度

定义3：设$(X,Y)$是二维随机变量，$F(x,y)$是其分布函数，若存在非负函数$f(x,y)$，使对任意实数$x、y$，有
$$
F(x,y)=\int_{-\infty}^{x}\int_{-\infty}^{y}f(u,v)dudv
$$
则称$(X,Y)$是**二维连续型随机变量**，函数$f(x,y)$称为$(X,Y)$的**概率密度**，或称为$X$和$Y$的**联合概率密度**

与一维情形类似，二维随机变量的概率密度$f(x,y)$具有以下性质：

* 非负性 $f(x,y)\geq 0$
* 归一性 $\int_{-\infty}^{+\infty}f(x,y)dxdy=1$
* 设$G\subset R^2$，则有$P\{(X,Y\}\in G=\iint_Gf(x,y)dxdy$
* 若$f(x,y)$在点$(x,y)$连续，则有$\frac{\partial ^2F(x,y)}{\partial x\partial y}=f(x,y)$

------

### 边缘分布

#### 边缘分布函数

定义1：设二维随机变量$(X,y)$的分布函数为$F(x,y)$，称$X$的分布函数$F_x(x)$为$(X,Y)$关于$X$的**边缘分布函数**；称$Y$的分布函数$F_Y(y)$为$(X,Y)$关于$Y$的**边缘分布函数**。因为
$$
F_X(x)=P\{X\leq x\}=p\{X\leq x,Y<+\infty\}，\\
F_Y(y)=P\{Y\leq y\}=P\{X<+\infty，Y\leq y\}，
$$
故可得$X，Y$的联合分布函数与他们各自的边缘分布函数之间的关系如下：
$$
F_X(x)=\lim_{y\to +\infty}F(x,y)，\\
F_X(x)=\lim_{y\to +\infty}F(x,y).
$$

#### 边缘分布律

定义2：若$(X,Y)$为二维离散型随机变量，其分布律为$P\{X=x_i，Y=y_j\}=p_{ij}，i，j=1，2，...$

称$X$的分布律为$(X,Y)$关于$X$的**边缘分布律**；称$Y$的分布律为$(X,Y)$关于$Y$的**边缘分布律**，且
$$
P\{X=x_i\}=p_{i.}=\sum_jp_{ij}，i=1，2，... \\
P\{Y=y_j\}=p_{.j}=\sum_jp_{ij}，j=1，2，...
$$
事实上，
$$
P\{X=x_i\}=P\{(X=x_i)\cap [\cup_j(Y=y_j]\}\\
=P\{\cup_j[(X=x_i)\cap (Y=y_j)]\}\\
=\sum_jP\{X=x_i，Y=y_j\}=\sum_jP_{ij}，
$$

#### 边缘概率密度

定义3：若$(X,Y)$是二维连续型随机变量，其概率密度为$f(x,y)$，则称$X$的概率密度$f_X(x)$为$(X,Y)$关于$X$的**边缘概率密度**；称$Y$的概率密度$f_Y(y)$为$(X,Y)$关于$Y$的**边缘概率密度**

定义4：设有界区域$D\subset R^2$，其面积记为$S_D$，若二维随机变量$(X,Y)$的概率密度为
$$
f(x,y)=\left\{
\begin{aligned}
\frac{1}{S_D}，(x,y)\in D，\\
0，其他
\end{aligned}
\right.
$$
则称$(X,Y)$在$D$上**服从二维均匀分布**

容易得到，若$G\subset D$，则有
$$
P\{(X,Y)\in G\}=\frac{S_G}{S_D}
$$
上式表明，如果二维随机变量$(X,Y)$在$D$上服从均匀分布，则其落在$D$上任何子区域内的概率与该子区域的面积成正比，而与该子区域的具体位置无关

### 随机变量的独立性

定义1：设$(X,Y)$是二维随机变量，其分布函数与边缘分布函数分别为$F(x,y)、F_X(x)、F_Y(y)$。若对于任意的实数对$(x,y)$，事件“$\{X\leq x\}$”和事件“$\{Y\leq y\}$”相互独立，即有
$$
P\{X\leq x，Y\leq y\}=P\{X\leq x\}P\{Y\leq y\}，
$$
也就是
$$
F(x,y)=F_X(x)F_Y(y)，
$$
则称**随机变量$X$和$Y$相互独立**

如果$(X,Y)$是二维连续型随机变量，其概率密度与边缘概率密度分别为$f(x,y)、f_X(x)、f_Y(y)$，则$X$和$Y$相互独立的充要条件是
$$
f(x,y)=f_X(x)f_Y(y)
$$
如果(X,Y)是二维离散型随机变量，其分布律与边缘分布律分别为$p_{ij}、P_{i.}、P_{.j}$，则$X$和$Y$相互独立的充要条件是对$(X,Y)$的所有可能取值$(x_j,y_j)，i，j=1，2，...$，有
$$
P\{X=x_i,Y=y_j\}=P\{X=x_i\}P\{Y=y_j\}，
$$
即
$$
P_{ij}=P_{i.}P_{.j}，i，j=1，2，...
$$

------

### 条件分布

#### 离散型随机变量的条件分布律

设离散型随机变量$(X,Y)$的分布律为
$$
P\{X=x_i,Y=y_j\}=p_{ij}，i，j=1，2，...
$$
若$P\{Y=y_j\}>0$，则在事件$\{Y=y_j\}$发生的条件下，事件$\{X=x_i\}$的条件概率为
$$
P\{X=x_i\mid Y=y_j\}=\frac{P\{X=x_i,Y=y_j\}}{P\{Y=y_j\}}=\frac{p_{ij}}{p_{.j}}，i=1，2，...
$$
显然，满足分布律的性质：

* $P\{X=x_i\mid Y=y_j\}\geq 0，i=1，2，...；$
* $\sum_{i=1}^{+\infty}P\{X=x_i\mid Y=y_j\}=1.$

定义1：设离散型随机变量$(X,Y)$的分布律为
$$
P\{X=x_i,Y=y_j\}=p_{ij}，i，j=1，2，...
$$
对固定的$j$，若$P\{Y=y_j\}>0$，则称
$$
P\{X=x_i\mid Y=y_j\}=\frac{p_{ij}}{p_{.j}}，i=1，2，...
$$
为在$Y=y_j$的条件下，随机变量$X$的**条件分布律**

同样，对固定的$i$，若$P\{X=x_i\}>0$，则称
$$
P\{Y=y_j\mid X=x_j\}=\frac{p_{ij}}{p_{i.}}，j=1，2，...
$$
为在$X=x_j$的条件下，随机变$Y$的**条件分布律**

#### 连续型随机变量的条件概率密度

设$(X,Y)$为二维连续型随机变量，则对任意的实数$x$或$y$，有$P\{X=x\}=0$或$P\{Y=y\}=0$，故条件分布律的讨论方法不适用于连续型随机变量。下面我们用极限的方法来处理连续型情形

定义2：给定实数$y$及任意$\varepsilon >0$，若$P\{y<Y\leq y+\varepsilon\}>0$，且对任意实数$x$，极限
$$
\lim_{\varepsilon \to 0}P\{X\leq x\mid y<Y\leq y+\varepsilon\}=\lim_{\varepsilon \to 0}\frac{P\{X\leq x,y<Y\leq y+\varepsilon\}}{P\{y<Y\leq y+\varepsilon\}}
$$
存在，则称此极限为在“$Y=y$的条件下，随机变量$X$的**条件分布函数**。记作$F_{X\mid Y}(x\mid y)$或$P\{X\leq x\mid Y=y\}$。

设$(X,Y)$的概率密度函数为$f(x,y)$，$Y$的边缘概率密度为$f_Y(y)$，则上式即是
$$
F_{X\mid Y}(x\mid y)=\lim_{\varepsilon \to 0}\frac{\int_{-\infty}^x\int_y^{y+\varepsilon}f(u,v)dudv}{\int_y^{y+\varepsilon}f_Y(v)dv}
$$
若$f(x,y)$,$f_Y(y)$连续，且$f_Y(y)>0$，由积分中值定理可得
$$
F_{X\mid Y}(x\mid y)=\lim_{\varepsilon \to 0}\frac{\int_{-\infty}^xf(u,y+\vartheta_1 \varepsilon)du}{f_Y(y+\vartheta_2 \varepsilon)}，0<\vartheta_i<1，i=1，2
$$
故
$$
F_{X\mid Y}(x\mid y)=\frac{\int_{-\infty}^xf(u,y)du}{f_Y(y)}=\int_{-\infty}^x\frac{f(u,v)}{f_Y(y)}du
$$
若记$f_{X\mid Y}(x\mid y)$为在“$Y=y$”条件下，**X的条件概率密度**，则
$$
f_{X\mid Y}(x\mid y)=\frac{\partial F_{X\mid Y}(x\mid y)}{\partial x}=\frac{f(x,y)}{f_Y(y)}
$$
类似地，若$f_X(x)>0$在“$X=x$”条件下，**Y的条件概率密度**为
$$
f_{Y\mid X}(y\mid x)=\frac{\partial F_{Y\mid X}(y\mid x)}{\partial y}=\frac{f(x,y)}{f_X(x)}
$$
由概率密度的性质，在“$X=x_0$”条件下，随机事件$\{a<Y\leq b\}$的概率为
$$
P\{a<Y\leq b\mid X=x_0\}=\int_a^bf_{Y\mid X}(y\mid x_0)dy
$$

------

### 两个随机变量函数的分布

#### $(X,Y)$为离散型随机变量

设离散型随机变量$(X,Y)$的分布律为
$$
P\{X=x_i,Y=y_j\}=p_{ij}，i，j=1，2，...
$$
则$X,Y$的函数$Z=g(X,Y)$也是离散型随机变量，其分布律为
$$
P\{Z=z_k\}=\sum_{(x_i,y_j)\epsilon S-K}P\{X=x_i,Y=y_j\}，k=1，2，...
$$
其中
$$
S_k=\{(x_i,y_j:g(x_i,y_j)=z_k\}
$$

#### $(X,Y)$为连续型随机变量

设连续性随机变量$(X,Y)$的概率密度为$f(x,y)$，当$Z=g(X,Y)$为连续型随机变量时，它的分布函数为
$$
F_z(z)=P\{Z\leq z\}=P\{g(X,Y)\leq z\}=\iint_{g(x,y)\leq z}f(x,y)dxdy，
$$
于是，$Z$的概率密度为
$$
f_Z(z)=F'_Z(z)
$$
下面介绍两类常见的随机变量函数的分布
1. Z=X+Y的分布
设$(X,Y)$的概率密度为$f(x,y)$，随机变量$Z=X+Y$的分布函数为
$$
F_Z(z)=p\{Z\leq z\}=P\{X+Y\leq z\} \\
=\iint_{x+y\leq z}f(x,y)dxdy=\int_{-\infty}^{+\infty}dx\int_{-\infty}^{z-x}f(x,y)dy \\
=\int_{-\infty}^{+\infty}dx\int_{-\infty}^{z}f(x,v-x)dv(令y=v-x) \\
=\int_{-\infty}^z[\int_{-\infty}^{+\infty}f(x,v-x)dx]dv
$$
两端对$z$求导，得$Z=X+Y$的概率密度为
$$
f_Z(z)=\int_{-\infty}^{+\infty}f(x,z-x)dx
$$
同理可得，$f_Z(z)=\int_{-\infty}^{+\infty}f(z-y,y)dy$
特别地，如果$X,Y$相互独立，上述公式变为 $f_Z(z)=\int_{-\infty}^{+\infty}f_X(x)f_Y(z-x)dx$
或$f_Z(z)=\int_{-\infty}^{+\infty}f_X(z-y)f_Y(y)$，该两式称为**卷积公式**
如果$X\sim N(\mu_1,\sigma_1^2)，Y\sim N(\mu_2,\sigma_2^2)，且与$$X$与$Y$相互独立，则
$$
X+Y\sim N(\mu_1 +\mu_2,\sigma_1^2+\sigma_2^2)
$$

一般地，如果$X_1,X_2,...,X_n$相互独立，且$X_i\sim N(\mu ,\sigma_i^2)$，还可以得到$\sum_{i=1}^{n}\alpha_iX_i\sim N(\sum_{i=1}^n\alpha_i\mu_i,\sum_{i=1}^n\alpha_1^2\sigma_1^2)$

其中$\alpha_i,i=1,2,...,n$为不全为零的实线。即：有限个相互独立的正态随机变量的线性组合仍然服从正态分布。

2.极大值与极小值的分布

设随机变量$X_1,X_2,...,X_n$相互独立，它们的分布函数分别记为$F_i(X_i),i=1,2,...,n$，记$M=max(X_1,X_2,...,X_n)$，$N=min(X_1,X_2,...,X_n)$，它们分别分$X_1,X_2,...,X_n$的**极大值**与**极小值**

因为事件$\{M\leq z\}$与事件$\{X_1,X_2,...,X_n都不大于z\}$等价，故$M$的分布函数为
$$
F_M(z)=P\{M\leq z\}=P\{max(X_1,X_2,...,X_n\leq z\}\\
=P\{X_1\leq z,X_2\leq z,...,X_n\leq z\}\\
=P\{X_1\leq z\}P\{X_2\leq z\}...P\{X_n\leq z\}\\
=F_1(z)F_2(z)...F_n(z)
$$
因为事件$\{N>z\}$与事件$\{X_1,X_2,...,X_n都大于z\}$等价，故$N$的分布函数为
$$
F_N(z)=P\{N\leq z\}=1-P\{min(X_1,X_2,...,X_n)>z\}\\
=1-P\{X_1>z,X_2>z,...,X_n>z\}\\
=1-P\{X_1>z\}P\{X_2>z\}...P\{X_n>z\}\\
=1-[1-F_1(z)][1-F_2(z)]...[1-F_n(z)]
$$
特别地，若$X_1,X_2,...,X_n$相互独立且具有相同的分布函数$F(x)$,则
$$
F_M(z)=[F(z)]^n,\\
F_N(z)=1-[1-F(z)]^n
$$

------

## 随机变量的数字特征

### 数学期望

#### 数学期望的定义

定义1：设离散型随机变量$X$的分布律为$P\{X=x_i\}=p_i,i=1,2,...$若$\sum_{i=1}^{+\infty}\mid x_i\mid p_i<+\infty$，则称
$$
E(X)=\sum_{i=1}^{+\infty}x_ip_i
$$


为**离散型随机变量$X$的数学期望**或**均值**

若随机变量$X$为连续型随机变量，其概率密度为$f(x)$，则$f(x)dx$的作用与离散型随机变量中的$p_i$相类似，于是，连续型随机变量的数学期望的定义可给出如下

定义2：设连续型随机变量$X$的概率密度为$f(x)$，若$\int_{-\infty}^{+\infty}\mid x\mid f(x)dx<+\infty$，则称
$$
E(X)=\int_{-\infty}^{+\infty}xf(x)dx
$$
为**连续型随机变量$X$的数学期望**或**均值**

#### 随机变量函数的数学期望

定理1：设$Y$是随机变量$X$的函数$Y=g(X)$，其中$g(x)$是连续函数

1. 若$X$是离散型随机变量，其分布律为$P\{X=x_i\}=p_i,i=1,2,...$如果$\sum_{i=1}^{+\infty}\mid g(x_i)\mid p_i<+\infty$，则有$E(Y)=E[g(X)]=\sum_{i=1}^{+\infty}g(x_i)p_i$
2. 若$X$是连续型随机变量，其概率密度是$f(x)$，如果$\int_{-\infty}^{+\infty}\mid g(x)\mid f(x)dx<+\infty$，则有$E(Y)=E[g(X)]=\int_{-\infty}^{+\infty}g(x)f(x)dx$

定理2：设随机变量$Z$是二维随机变量$(X,Y)$的连续函数$Z=g(X,Y)$

1. 若$(X,Y)$是离散型随机变量，其分布律为$P\{X=x_i,Y=y_j\}=P_{ij},i=1,2,...,j=1,2,...$如果$\sum_{i=1}^{+\infty}\sum_{j=1}^{+\infty}\mid g(x_i,y_j)\mid p_{ij}<+\infty$，则$E(Z)$存在，且有$E(Z)=E[g(X,Y)]=\sum_{i=1}^{+\infty}\sum_{j=1}^{+\infty}g(x_i,y_j)p_{ij}$
2. 若$(X,Y)$是连续型随机变量，其概率密度为$f(x,y)$，如果$\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}\mid g(x,y)\mid f(x,y)dxdy<+\infty$，则$E(Z)$存在，且有$E(Z)=E[g(X,Y)]=\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}g(x,y)f(x,y)dxdy$

#### 数学期望的性质

随机变量的数学期望具有以下重要性质：

1. 设$C$为常数，则$E(C)=C$
2. 设$C$为常数，则$E(CX)=CE(X)$
3. 设$X、Y$是两个随机变量，则有$E(X+Y)=E(X)+E(Y)$
4. 设$X、Y$是相互独立的随机变量，则有$E(XY)=E(X)E(Y)$

------

### 方差

定义1：设$X$是随机变量，若$E\{[X-E(X)]^2\}$存在，则称
$$
D(X)=E\{[X-E(X)]^2\}
$$
为$X$**方差**，称$\sigma (X)=\sqrt{D(X)}$为$X$的**标准差**或**均方差**

方差是随机变量$X$的函数的数学期望，若$X$是离散型随机变量，其分布律为
$$
P\{X=x_i\}=p_i,i=1,2,...,
$$
则
$$
D(X)=\sum_{i=1}^{+\infty}[x_i-E(X)]^2p_{i}
$$
若$X$是连续型随机变量，其概率密度为$f(x)$，则
$$
D(X)=\int_{-\infty}^{+\infty}[x-E(X)]^2f(x)dx
$$
方差的常用计算公式为
$$
D(X)=E(X^2)-[E(X)]^2
$$
对于方差，可以证明它有以下**重要性质**：

1. 设$C$为常数，则$D(C)=0$
2. 设$C$为常数，则$D(CX)=C^2D(X)$
3. 设$X、Y$为随机变量，有$D(X\pm Y)=D(X)+D(Y)\pm 2E\{[X-E(X)][Y-E(Y)]\}$若$X$与$Y$相互独立，则$D(X\pm Y)=D(X)+D(Y)$
4. **切比雪夫不等式** 设随机变量$X$的数学期望$E(X)$和方差$D(X)$存在，则对任意常数$\varepsilon >0$，有$P\{\mid X-E(X)\mid \geq \varepsilon\}\leq \frac{D(X)}{\varepsilon^2}$或$P\{\mid X-E(X)\mid <\varepsilon\}\geq 1-\frac{D(X)}{\varepsilon^2}$
5. $D(X)=0$的充要条件是$X$以概率1取常数$E(X)$，即$D(X)=0\Leftrightarrow P\{X=E(X)\}=1$

#### 常用随机变量的期望和方差

| 分布名称 |                       分布律或概率密度                       |      数学期望       |          方差          |
| :------: | :----------------------------------------------------------: | :-----------------: | :--------------------: |
| 二项分布 |           $p_k=C_n^kp^k(1-p)^{n-k},k=0,1,2,...,n$            |        $np$         |       $np(1-p)$        |
| 泊松分布 |      $p_k=\frac{\lambda^k}{k!}e^{-\lambda},k=0,1,2,...$      |      $\lambda$      |       $\lambda$        |
| 均匀分布 | $f(x)=\left\{\begin{aligned}\frac{1}{b-a},a<x<b\\0,其他\\\end{aligned}\right.$ |   $\frac{a+b}{2}$   |  $\frac{(b-a)^2}{12}$  |
| 指数分布 | $f(x)=\left\{\begin{aligned}\lambda e^{-\lambda x},x\geq 0\\0,x<0\\\end{aligned}\right.$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda ^2}$ |
| 正态分布 | $f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ |        $\mu$        |       $\sigma^2$       |

-----

### 协方差与相关系数

#### 协方差

定义1：设$(X,Y)$为二维随机变量，若$E\{[X-E(X)][Y-E(Y)]\}$存在，则称之为随机变量$X$与$Y$的**协方差**，记为$Cov(X,Y)$，即
$$
Cov(X,Y)=E\{[X-E(X)][Y-E(Y)]\}
$$
易得协方差的常用计算公式
$$
Cov(X,Y)=E(XY)-E(X)E(Y)
$$
协方差具有下述**性质**：

1. $Cov(X,Y)=Cov(Y,X)$
2. $Cov(X,a)=0$
3. $Cov(X,X)=D(X)$
4. $Cov(aX,bY)=abCov(X,Y)$
5. $Cov(X_1\pm X_2,Y)=Cov(X_1,Y)\pm Cov(X_2,Y)$，$Cov(X_1+X_2,Y_1+Y_2)=Cov(X_1,Y_1)+Cov(X_1,Y_2)+Cov(X_2,Y_1)+Cov(X_2,Y_2)$
6. $D(X\pm Y)=D(X)+D(Y)\pm 2Cov(X,Y)$
7. 若$X$与$Y$相互独立，则$Cov(X,Y)=0$

#### 相关系数

定义2：设$(X,Y)$为二维随机变量，其协方差及方差$D(X)$与$D(Y)$都存在，且$D(X)>0$，$D(Y)>0$，则称
$$
\rho_{XY}=\frac{Cov(X,Y)}{\sqrt{D(X)D(Y)}}
$$
为随机变量$X$与$Y$的**相关系数**

显然，可以写成
$$
\rho_{XY}=E(\frac{X-E(X)}{\sqrt{D(X)}}\cdot\frac{Y-E(Y)}{\sqrt{D(Y)}})=Cov(\frac{X-E(X)}{\sqrt{D(X)}},{\frac{Y-E(Y)}{\sqrt{D(Y)}}})
$$
相关系数具有如下**性质**：

1. 若$X$与$Y$相互独立，则$\rho_{XY}=0$
2. $\mid\rho_{XY}\mid\leq 1$
3. $\mid\rho_{XY}\mid=1$的充要条件是$X$与$Y$以概率1线性相关，及存在常数$a(a\neq 0),b$，使得$P\{Y=aX+b\}=1$

定义3：若随机变量$X$与$Y$的相关系数存在，且$\rho_{XY}=0$，则称$X$与$Y$**不相关**；若$\rho_{XY}=1$，则称$X$与$Y$**完全不相关**；若$\rho_{XY}=-1$，则称$X$与$Y$**完全负相关**。有相关系数的性质（1）知，若随机变量$X$与$Y$相互独立，则$X$与$Y$不相关，反之不然。

------

### 矩与协方差矩阵

#### 矩

定义1：设$X$与$Y$是随机变量，若
$$
E(X^k),k=1,2,...
$$
存在，则称之为$X$的$k$阶**原点矩**，简称为$k$阶矩，若
$$
E\{[X-E(X)]^k\},k=2,3,...
$$
存在，则称之为$X$的$k$阶**中心矩**。若
$$
E\{[X-E(X)]^k[Y-E(Y)]^l\},k,l=1,2,...
$$
存在，则称之为$X$与$Y$的$k+l$阶**混合中心矩**

显然，$k$阶原点矩、$k$阶中心矩、$k+l$阶混合中心矩分别可视为期望、方差、协方差的推广

#### 协方差矩阵

设$X=(X_1,X_2,...,X_n)^T$为$n$维随机变量，称矩阵
$$
C=(c_{ij})_{n\times n}=\begin{pmatrix}
c_{11}&c_{12}&...&c_{1n}\\
c_{21}&c_{22}&...&c_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
c_{n1}&c_{n2}&...&c_{nn}\\
\end{pmatrix}
$$
为$n$维随机变量$X$的**协方差矩阵**，也记为$D(X)$，其中
$$
c_{ij}=Cov(X_i,X_j),i,j=1,2,...,n
$$
为$X$的分量$X_i$和$X_j$的协方差（设它们都存在）

协方差矩阵具有下列**性质**：

1. **对称性** $C^T=C$
2. **非负定性** 对任意$n$维向量$x=(x_1,x_2,...,x_n)^T$，有$X^TCx\geq 0$

------

 ### n维正态分布

定义1：若$C$是$n$阶正定矩阵，记其逆矩阵$C^{-1}$，$C$的行列式为$\mid C\mid$，$\mu =(\mu_1,\mu_2,...\mu_n)^T$为任意的实值列向量，如果$n$维随机变量$X=(X_1,X_2,...,X_n)^T$的概率密度为
$$
f(x)=\frac{1}{(2\pi)^{\frac{n}{2}}\mid C\mid^{\frac{1}{2}}}exp\{-\frac{1}{2}(x-\mu)^TC^{-1}(x-\mu)\}
$$
则称$X$服从**$n$维正态分布**，简记为$N(\mu ,C)$.这里$x=(x_1,x_2,...,x_n)^T$是一个$n$维列向量

特别地，当$n=2$，且
$$
\mu =\begin{pmatrix}
\mu_1 \\
\mu_2 \\
\end{pmatrix},C=\begin{pmatrix}
\sigma_1^2&\rho \sigma_1\sigma_2 \\
\rho \sigma_1 \sigma_2&\sigma_2^2 \\
\end{pmatrix}
$$


时，代入上式即得二维正态分布的概率密度函数

------

## 大数定律与中心极限定理

### 大数定律

#### 依概率收敛

定义1：设$Y_1,Y_2,...,Y_n,...$为随机变量序列，$a$是一常数，若对任意的$\varepsilon >0$，有
$$
\lim_{n \to+\infty}P\{\mid Y_n-a\mid <\varepsilon\}=1
$$
则称$Y_1,...,Y_n,...$**依概率收敛**于$a$，记为
$$
Y_n\xrightarrow[\qquad]{p}a
$$
上式说明序列$Y_1,Y_2,...,Y_n,...$与常数$a$任意接近事件的概率无限趋向于1，但并不是说序列$Y_1,Y_2,...,Y_n,...$趋向于$a$

#### 几个常用的大数定律

定理1（切比雪夫定理的特殊情况）设随机变量$X_1,...,X_n,...$相互独立且具有相同的数学期望和方差：$E(X_k)=\mu,D(X_k)=\sigma^2(k=1,2,...)$.做前$n$项随机变量的算术平均
$$
\bar{X}=\frac{1}{n}\sum_{k=1}^nX_k,
$$
则
$$
\bar{X}\xrightarrow[\quad]{P}\mu,
$$
即$\forall \varepsilon>0$，有
$$
\lim_{n\to+\infty}P\{\mid\bar{X}-\mu\mid<\varepsilon\}=1
$$
定理2（伯努利大数定律）：设事件$A$的概率为$p,n_A$是$n$次独立试验中事件$A$发生的次数，则
$$
\frac{n_A}{n}\xrightarrow[\quad]{P}p
$$


即得
$$
\lim_{n\to+\infty}P\{\mid\frac{n_A}{n}-p\mid<\varepsilon\}=1
$$
定理3（辛钦大数定律）设随机变量$X_1,...,X_n,...$相互独立，服从同一分布，且具有数学期望$E(X_k)=\mu,k=1,2,...$则
$$
\bar{X}=\frac{1}{n}\sum_{k=1}^{n}X_k\xrightarrow[\quad]{P}\mu
$$
推论1：设$X_1,...,X_n,...$为独立同分布的随机变量序列，$E(X_1^k)<+\infty$,则
$$
\frac{1}{n}\sum_{i=1}^{n}X_i^k\xrightarrow[\quad]{P}E(X_1^k)
$$

------

### 中心极限定理

设$X_1,...,X_n,...$是相互独立同分布的随机变量，对任意$i$有$E(X_i)=\mu,D(X_i)=\sigma^2>0$，记$S_n=X_1+X_2+...+X_n$为部分和序列，
$$
Z_n=\frac{S_n-E(S_n)}{\sqrt{D(S_n)}}
$$
为部分和$S_n$的标准化随机变量

定理1（独立同分布中心极限定理）设随机变量$X_1,...,X_n,...$相互独立，服从同一分布，具有相同的期望和非零的方差，部分和的标准化变量$Z_n$的分布函数为$F_n(x)$，则
$$
\lim_{n\to+\infty}F_n(x)=\lim_{n\to+\infty}P\{Z_n\leq x\}=\int_{-\infty}^x\frac{1}{\sqrt{2\pi}}e^{-\frac{t^2}{2}}dt=\phi(x)
$$
当$n$充分大时，$n$个独立同分布随机变量之和近似服从正态分布，即
$$
Z_n=\frac{\sum_{i=1}^nX_i-n\mu}{\sigma\sqrt{n}}
$$
近似服从$N(0,1)$，或
$$
\frac{\frac{1}{n}\sum_{i=1}^nX_i-\mu}{\sigma/\sqrt{n}}
$$
推论1：在定理1条件下，当$n$充分大时，有
$$
P\{\sum_{i=1}^nX_i\leq x\}\approx\phi(\frac{x-n\mu}{\sqrt{n}\sigma})
$$
定理2（棣莫弗-拉普拉斯定理）设随机变量$Y_n,n=1,2,...$，服从二项分布$B(n,p)$，则对任意区间$(a,b]$，恒有
$$
\lim_{n\to+\infty}P\{a<\frac{Y_n-np}{\sqrt{np(1-p)}}\leq b\}=\int_{a}^b\frac{1}{\sqrt{2\pi}}e^{-\frac{t^2}{2}}dt=\phi(b)-\phi(a)
$$

------

## 数理统计的基本概念

### 总体与样本

定义1：从总体$X\sim F(x)$中抽取$n$个个体，得到$n$个随机变量$X_1,X_2,...,X_n$，若

1. $X_1,X_2,...,X_n$与总体$X$同分布
2. $X_1,X_2,...,X_n$相互独立，

则称$X_1,X_2,...,X_n$为从总体$X$（或从$F$）得到的容量为$n$的**简单随机样本**，简称**样本**，它们的观察值$x_1,x_2,...,x_n$称为**样本观察值**，抽取样本的过程称为**抽样**

#### 统计量

定义2：设$X_1,X_2,...,X_n$是来自总体$X$的一个样本，若样本的函数$g(X_1,X_2,...,X_n)$中不含未知参数，则称$g(X_1,X_2,...,X_n)$是一**统计量**

若$x_1,x_2,...,x_n$是样本$X_1,X_2,...,X_n$的观察值，则$g(x_1,x_2,...,x_n)$就是$g(X_1,X_2,...,X_n)$的观察值

下面列举一些常用的统计量：

* **样本均值** $\bar{X}=\frac{1}{n}\sum_{i=1}^{n}X_i$
* **样本方差** $S^2=\frac{1}{n-1}\sum_{i=1}^n(X_i-\bar{X})^2$
* **样本标准差** $S=\sqrt{S^2}=\sqrt{\frac{1}{n-1}\sum_{i=1}^n(X_i-\bar{X})^2}$
* **样本$k$阶中心矩** $B_k=\frac{1}{n}\sum_{i=1}^n(X_i-\bar{X})^k,k=2,3,...$
* **样本$k$阶(原点)矩** $A_k=\frac{1}{n}\sum_{i=1}^nX_i^k,k=1,2,...$

定义3：设$X_1,X_2,...,X_n$是来自总体$X$的样本，$X_1,X_2,...,X_n$的第$i$个顺序统计量$X_{(i)}$是满足如下条件的样本函数：每得到一组样本值$x_1,x_2,...,x_n$，将它们按从小到大的顺序排列为
$$
x_{(1)}\leq x_{(2)}\leq ...\leq x_{(n)}，
$$
其中第$i$和数$x_{(i)}$为$X_{(i)}$的观察值.$X_{(1)},X_{(2)},...,X_{(n)}$称为样本$X_1,X_2,...,X_n$的**顺序统计量**，其中$X_{(1)}$称为**最小顺序统计量**，$X_{(n)}$称为**最大顺序统计量**

* $E(\bar{X})=E(X)$
* $D(\bar{X})=\frac{D(X)}{n}$
* $E(S^2)=D(X)$

#### 经验分布函数

总体$X$的分布函数一般是未知的，当给定样本值$x_1,x_2,...,x_n$时，可以构造一个样本分布函数，称为经验分布函数。当样本容量$n$较大时，我们可用经验分布函数来推断或代替总体的分布函数

定义4：设$x_1,x_2,...,x_n$是来自总体$X\sim F(x)$的样本值，将$x_1,x_2,...,x_n$从小到大排列得
$$
x_{(1)}\leq x_{(2)}\leq...\leq x_{(n)}，
$$
令
$$
F_{n}(x)=\left\{
\begin{aligned}
0,x<x_{(1)},\\
k/n\quad,x_{(k)}\leq x<x_{(k+1)},k=1,...,n-1,\\
1,x\geq x_{(n)},
\end{aligned}
\right.
$$
称$F_n(x)$为**经验分布函数**

经验分布函数具有如下**性质**：

1. 经验分布函数是一分布函数
2. $P\{\underset{n\to+\infty\, x\in R}{\lim}sup\mid F_n(x)-F(x)\mid=0\}=1$

------

### 常用统计量的分布

#### $\chi^2$分布

定义1：设$X_1,X_2,...,X_n$为相互独立且均服从$N(0,1)$的随机变量，称
$$
\chi^2=X_1^2+X_2^2+...+X_n^2
$$
服从自由度为$n$的$\chi^2$**分布**，记为$\chi^2\sim\chi^2(n)$

性质1：设$\chi^2\sim\chi^2(n)$，则$E(\chi^2)=n,D(\chi^2）=2n)$

性质2：设$\chi_1^2\sim\chi_1^2(n)$，$\chi_2^2\sim\chi_2^2(n)$，并且$\chi_1^2$与$\chi_2^2$相互独立，则$\chi_1^2+\chi_2^2\sim\chi^2(n_1+n_2)$

定义2：设$\chi^2\sim\chi^2(n)$，对于给定的$\alpha(0<\alpha<1)$，称满足条件
$$
P\{\chi^2>\chi_{\alpha}^2(n)\}=\int_{\chi_\alpha^2(n)}^{+\infty}f(x)dx=\alpha
$$
的点$\chi_{\alpha}^2(n)$为$\chi^2(n)$分布的上**$\alpha$分位点**

#### t分布

定义3：$X\sim N(0,1),Y\sim\chi^2(n)$，且$X$与$Y$相互独立，称随机变量
$$
t=\frac{X}{\sqrt{Y/n}}
$$
服从自由度为$n$的**$t$分布**，记为$t\sim t(n)$

性质3：

1. $t(n)$分布的概率密度$f(x)$具有对称性，即$f(x)=f(-x)$
2. $t(n)$分布的概率密度$f(x)$的极限是标准正态分布的概率密度，即$\lim_{n\to +\infty}f(x)=\varphi(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$

定义4：设$t\sim t(n)$，对于给定的$\alpha(0<\alpha<1)$，称满足条件
$$
P(t>t_{\alpha}(n))=\int_{t_{\alpha}(n)}^{+\infty}f(x)dx=\alpha
$$
的点$t_{\alpha}(n)$为$t(n)$分布的上**$\alpha$分位点**

#### $F$分布

定义5：设$X\sim\chi^2(n_1),Y\sim\chi^2(n_2)$，且$X$与$Y$相互独立，称随机变量
$$
F=\frac{X/n_1}{Y/n_2}
$$
服从第一自由度为$n_1$，第二自由度为$n_2$的**$F$分布**，记作$F\sim F(n_1,n_2)$

性质4：若$F\sim F(n_1,n_2)$，则$1/F\sim F(n_2,n_1)$

定义6：$F\sim F(n_1,n_2)$，对给定的$\alpha(0<\alpha<1)$，称满足条件
$$
P\{F>F_{\alpha}(n_1,n_2)\}=\int_{F_{\alpha(n_1,n_2)}}^{+\infty}f(x)dx=\alpha
$$
的点为$F_{\alpha}(n_1,n_2)$为$F$分布的上**$\alpha$分位点**

------

### 抽样分布

定理1：设$X_1,X_2,...,X_n$是来自正态总体$N(\mu,\sigma^2)$的样本，$\bar{X}$为样本均值，则
$$
\bar{X}\sim(\mu,\frac{\sigma^2}{n}),\frac{\bar{X}-\mu}{\sigma/\sqrt{n}}\sim N(0,1)
$$
定理2：设$X_1,X_2,...,X_n$是来自正态总体$N(\mu,\sigma^2)$的样本，则
$$
\frac{1}{\sigma^2}\sum_{i=1}^n(X_i-\mu)^2\sim\chi^2(n)
$$
定理3：设$X_1,X_2,...,X_n$是来自正态总体$N(\mu,\sigma^2)$的样本，则

1. $\frac{(n-1)S^2}{\sigma^2}=\frac{1}{\sigma^2}\sum_{i=1}^{n}(X_i-\bar{X})^2\sim\chi^2(n-1)$
2. $\bar{X}$与$S^2$相互独立

定理4：设$X_1,X_2,...,X_n$是来自正态总体$N(\mu,\sigma^2)$的样本，则
$$
\frac{\bar{X}-\mu}{S/\sqrt{n}}\sim t(n-1)
$$
定理5：设$(X_1,X_2,...,X_{n1}),(Y_1,Y_2,...,Y_{n2})$分别是来自正态总体$N(\mu_1,\sigma_1^2)$和$N(\mu_2,\sigma_2^2)$的样本，且它们相互独立，则

1. $\frac{S_1^2/\sigma_1^2}{S_2^2/\sigma_2^2}\sim F(n_1-1,n_2-1)$
2. 当$\sigma_1^2=\sigma_2^2=\sigma^2$时，$\frac{\bar{X}-\bar{Y}-(\mu_1-\mu_2)}{S_\omega\sqrt{1/n_1+1/n_2}}\sim t(n_1+n_2-2)$

------

## 参数估计

### 点估计

#### 矩估计法

引理1：设$X_1,X_2,...,X_n$是总体$X$的样本，则样本的$k$阶矩依概率收敛于总体的$k$阶矩，即
$$
A_k=\frac{1}{n}\sum_{i=1}^nX_i^k\xrightarrow[\quad]{P}E(X^k),k=1,2,...
$$
定义1：按样本$k$阶矩等于总体的$k$阶矩方法求出的$\hat{\theta_j},j=1,...,m$称为未知参数$\theta_j(j=1,...,m)$的**矩法估计**

#### 最大似然估计法

定义2：设总体$X$的分布函数为$F(x;\theta)，\theta$为待估参数($\theta$可以是未知参数向量)，$X_1,X_2,...,X_n$为来自总体$X$的样本，当$X$是离散型时，称$X_1,X_2,...,X_n$的联合分布律
$$
L(\theta)=\prod_{i=1}^np(x_i;\theta)
$$
为样本的**似然函数**，当$X$是连续型时，称$X_1,X_2,...,X_n$的联合概率密度
$$
L(\theta)=\prod_{i=1}^nf(x_i;\theta)
$$
为样本的**似然函数**

根据最大似然估计的思想，我们要选取$\hat{\theta}$，使似然函数$L(\theta)$达到最大，即
$$
L(x_1,x_2,...,x_n;\hat{\theta})=\underset{\theta\in\Theta}{max}L(x_1,x_2,...,x_n;\theta)
$$
定义3：若有$\hat{\theta}(x_1,x_2,...,x_n)$，使上式成立，则称$\hat{\theta}(x_1,x_2,...,x_n)$为$\theta$的**最大似然估计值**，称$\hat{\theta}(x_1,x_2,...,x_n)$为$\theta$的**最大似然估计量**，最大似然估计值与最大似然估计量都称为**最大似然估计**

解法一：

1. 做似然函数 $$L(\theta)=\prod_{i=1}^np(x_i;\theta)$$或$L(\theta)=\prod_{i=1}^nf(x_i;\theta)$
2. 做对数似然函数 $ln\,L(\theta)=\sum_{i=1}^nln\,p(x_i;\theta)$或$ln\,L(\theta)=\sum_{i=1}^nln\,f(x_i;\theta)$
3. 求导对数似然函数，并列对数似然方程$\frac{d}{d\theta}ln\,L(\theta)=0$
4. 解对数似然方程得参数$\theta$的最大似然估计

解法二：

记$x_{(1)}=min\{x_1,x_2,...,x_n\},x_{n}=max\{x_1,x_2,...,x_n\}$.

似然函数的非零区域与待估参数有关.在这种情况下，通常无法通过解似然方程或对数似然方程来获得参数的最大似然估计，这时可直接从定义出发求$\theta$的最大似然估计。

性质1（不变原则）设$\theta$的最大似然估计为$\hat{\theta},\theta$的函数$g(\theta)$具有单值反函数，则$g(\theta)$的最大似然估计为$g(\hat{\theta})$

------

### 估计量的评选标准

#### 无偏性

定义1：设$\hat{\theta}=\hat{\theta}(X_1,X_2,...,X_n)$是参数$\theta$的一个估计量，$\Theta$为参数$\theta$的所有可能取值集合，若有
$$
E(\hat{\theta})=\theta,\forall\theta\in\Theta
$$
则称$\hat{\theta}=\hat{\theta}(X_1,X_2,...,X_n)$为$\theta$的**无偏估计量**

### 有效性

定义2：设$\hat{\theta}_1=\hat{\theta}_1(X_1,X_2,...,X_n)$和$\hat{\theta}_2=\hat{\theta}_2(X_1,X_2,...,X_n)$都是参数$\theta$的无偏估计，若对一切$\theta\in\Theta$，有
$$
D(\hat{\theta}_1)\leq D(\hat{\theta}_2)
$$
并且至少存在一个$\theta\in\Theta$使不等号成立，则称估计量$\hat{\theta}_1$比$\hat{\theta}_2$**有效**

#### 相合性

定义3：设$\hat{\theta}_n=\hat{\theta}_n(X_1,X_2,...,X_n)$是待估参数$\theta$的估计量。若对任意的$\varepsilon>0$，有
$$
\underset{n\to+\infty}{\lim}P\{\mid\hat{\theta}_n-\theta\mid\geq\varepsilon\}=0
$$
则称$\hat{\theta}_n$是$\theta$的**相合估计量**或**一致估计量**

------

### 区间估计

定义1：设总体$X$的分布函数$F(x;\theta)$含有一个未知参数$\theta$，对于给定值$\alpha(0<\alpha<1)$，若由总体$X$的样本$X_1,X_2,...,X_n$确定的两个统计量$\underline{\theta}=\underline{\theta}(X_1,X_2,...,X_n)$和$\bar{\theta}=\bar{\theta}(X_1,X_2,...,X_n)(\underline{\theta}<\bar{\theta})$，满足
$$
P\{\underline{\theta}<\theta<\bar{\theta}\}=1-\alpha
$$
则称随机区间$(\underline{\theta},\bar{\theta})$是$\theta$的置信水平（或置信度）为$1-\alpha$的**置信区间**，$\underline{\theta}$和$\bar{\theta}$分别称为$\theta$的置信水平为$1-\alpha$的**置信下限**和**置信上限**

#### 正态总体参数的置信区间

|  待估参数  |      条件      |                置信水平为$1-\alpha$的置信区间                |
| :--------: | :------------: | :----------------------------------------------------------: |
|   $\mu$    | $\sigma^2$已知 |      $(\bar{X}\pm z_{\alpha/2}\frac{\sigma}{\sqrt{n}})$      |
|   $\mu$    | $\sigma^2$未知 |      $(\bar{X}\pm t_{\alpha/2}(n-1)\frac{S}{\sqrt{n}})$      |
| $\sigma^2$ |   $\mu$未知    | $(\frac{(n-1)S^2}{\chi^2_{\alpha/2}(n-1)},\frac{(n-1)S^2}{\chi^2_{1-\alpha/2}(n-1)})$ |

#### 两个正态总体参数的置信区间

|            待估参数             |            条件             |                置信水平未$1-\alpha$的置信区间                |
| :-----------------------------: | :-------------------------: | :----------------------------------------------------------: |
|          $\mu_1-\mu_2$          | $\sigma_1^2,\sigma_2^2$已知 | $(\bar{X}-\bar{Y}\pm z_{\alpha/2}\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}})$ |
|          $\mu_1-\mu_2$          | $\sigma_1^2,\sigma_2^2$未知 | $(\bar{X}-\bar{Y}\pm z_{\alpha/2}(n_1+n_2-2)S_{\omega}\sqrt{\frac{1}{n_1}+\frac{1}{n_2}})$ |
| $\frac{\sigma_1^2}{\sigma_2^2}$ |      $\mu_1,\mu_2$未知      | $(\frac{S_1^2}{S_2^2}\frac{1}{F_{\alpha/2(n_1-1,n_2-1)}},\frac{S_1^2}{S_2^2}\frac{1}{F_{1-\alpha/2(n_1-1,n_2-1)}})$ |

------

## 假设检验

### 正态总体参数的假设检验

#### 正态总体$N(\mu,\sigma^2)$均值的检验

**Z检验法(适用于$\sigma^2$已知)**

设总体$X\sim N(\mu,\sigma^2)$，其中方差$\sigma^2$已知，$X_1,X_2,...,X_n$是来自$X$的一个样本，现对均值$\mu$提出假设.$Z$检验法的检验步骤如下：

1. 根据题意，提出如下假设之一：$H_0:\mu=\mu_0,H_1:\mu\neq\mu_0;\\H_0:\mu\leq\mu_0,H_1:\mu>\mu_0;\\H_0:\mu\geq\mu_0,H_1:\mu<\mu_0$ 
2. 选取$Z$检验统计量：$Z=\frac{\bar{X}-\mu_0}{\sigma/\sqrt{n}}$
3. 对给定的$\alpha$，查表求临界点.对双边检验，查表求标准正态分布的的上分位点$z_{\alpha/2}$；对右边检验，查表求标准正态分布的上分位点$z_{\alpha}$；对左边检验，查表求$-z_\alpha$
4. 根据样本观察值计算$Z$的值：$z=\frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}$
5. 做出判断：对双边检验，当$\mid z\mid\geq z_{\alpha/2}$时，拒绝$H_0$，反之接受$H_0$；对右边检验，当$z\geq z_\alpha$时，拒绝$H_0$，反之接受$H_0$；对左边检验，当$z\leq -z_\alpha$时，拒绝$H_0$，反之接受$H_0$.

**$t$检验法(适用于$\sigma^2$未知)**

1. 根据题意，提出如下假设之下：$H_0:\mu=\mu_0,H_1:\mu\neq\mu_0;\\H_0:\mu\leq\mu_0,H_1:\mu>\mu_0;\\H_0:\mu\geq\mu_0,H_1:\mu<\mu_0$  
2. 选取$t$检验统计量：$t=\frac{\bar{X}-\mu_0}{S/\sqrt{n}}$
3. 对给定的$\alpha$，查表求临界点：对双边检验，查表求$t$分布的上分位点$t_{\alpha/2}(n-1)$；对右边检验，查表求$t$分布的上分位点$t_{\alpha}(n-1)$；对左边检验，查表求$-t_{\alpha}(n-1)$
4. 根据样本观察值计算$t$的值：$t=\frac{\bar{x}-\mu_0}{s/\sqrt{n}}$
5. 做出判断：对双边检验，当$\mid t\mid\geq t_{\alpha/2}(n-1)$时，拒绝$H_0$，反之接受$H_0$；对右边检验，当$t\geq t_{\alpha}(n-1)$时，拒绝$H_0$，反之接受$H_0$；对左边检验，当$t\leq -t_{\alpha}(n-1)$时，拒绝$H_0$，反之接受$H_0$，反之接受$H_0$；

#### 正态总体方差的假设检验

$\chi^2$检验法的检验步骤如下：

1. 根据题意，提出如下假设之一：$H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2\neq\sigma_0^2;\\H_0:\sigma^2\leq\sigma_0^2,H_1:\sigma^2>\sigma_0^2;\\H_0:\sigma^2\geq\sigma_0^2,H_1:\sigma^2<\sigma_0^2.$
2. 选取$\chi^2$检验统计量：$\chi^2=\frac{(n-1)S^2}{\sigma_0^2}$
3. 对给定的$\alpha$，查表求临界点：对双边检验，查表求$\chi^2$分布的两个上分位点$\chi_{1-\alpha/2}^2(n-1)$和$\chi_{\alpha/2}^2(n-1)$；对右边检验，查表求上分位点$\chi_\alpha^2(n-1)$；对左边检验，查表求上分位点$\chi_{1-\alpha}^2(n-1)$.
4.  根据样本观察值计算$\chi^2$的值：$\chi^2=\frac{(n-1)s^2}{\sigma_0^2}$
5. 对双边检验，当$\chi^2\leq\chi_{1-\alpha/2}^2(n-1)或\chi^2\geq\chi_{\alpha/2}^2(n-1)$时，拒绝$H_0$，反之接受$H_0$；对右边检验，当$\chi^2\geq\chi_{\alpha}^2(n-1)$时，拒绝$H_0$，反之接受$H_0$；对左边检验，当$\chi^2\leq\chi_{1-\alpha}^2(n-1)$时，拒绝$H_0$，反之接受$H_0$.



|         检验类型         |                             假设                             |                检验统计量                 |                            拒绝域                            |
| :----------------------: | :----------------------------------------------------------: | :---------------------------------------: | :----------------------------------------------------------: |
| 均值检验($\sigma^2$已知) | $H_0:\mu=\mu_0,H_1:\mu\neq\mu_0;\\H_0:\mu\leq\mu_0,H_1:\mu>\mu_0;\\H_0:\mu\geq\mu_0,H_1:\mu<\mu_0$ | $Z=\frac{\bar{X}-\mu_0}{\sigma/\sqrt{n}}$ | $\mid z\mid\geq z_{\alpha/2}\\z\geq z_\alpha\\z\leq -z_\alpha$ |
| 均值检验($\sigma^2$未知) | $H_0:\mu=\mu_0,H_1:\mu\neq\mu_0;\\H_0:\mu\leq\mu_0,H_1:\mu>\mu_0;\\H_0:\mu\geq\mu_0,H_1:\mu<\mu_0$ |   $t=\frac{\bar{x}-\mu_0}{s/\sqrt{n}}$    | $\mid t\mid\geq t_{\alpha/2}(n-1)\\t\geq t_{\alpha}(n-1)\\t\leq -t_{\alpha}(n-1)$ |
|         方差检验         | $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2\neq\sigma_0^2;\\H_0:\sigma^2\leq\sigma_0^2,H_1:\sigma^2>\sigma_0^2;\\H_0:\sigma^2\geq\sigma_0^2,H_1:\sigma^2<\sigma_0^2.$ |   $\chi^2=\frac{(n-1)S^2}{\sigma_0^2}$    | $\chi^2\leq\chi_{1-\alpha/2}^2(n-1)或\chi^2\geq\chi_{\alpha/2}^2(n-1)\\\chi^2\geq\chi_{\alpha}^2(n-1)\\\chi^2\leq\chi_{1-\alpha}^2(n-1)$ |



------

### 两个正态总体参数的假设检验

#### 两个正态总体均值差的假设检验

1. 根据题意，提出如下假设之一：$H_0:\mu_1-\mu_2=\delta,H_1:\mu_1-\mu_2\neq\delta;\\H_0:\mu_1-\mu_2\leq\delta,H_1:\mu_1-\mu_2>\delta;\\H_0:\mu_1-\mu_2\geq\delta,H_1:\mu_1-\mu_2<\delta.$
2. 选取$t$检验统计量$t=\frac{\bar{X}-\bar{Y}-\delta}{S_{\omega}\sqrt{1/n_1+1/n_2}}$
3. 对给定的$\alpha$，查表求临界点。对双边检验，查表求$t$分布的上分位点$t_{\alpha/2}(n_1+n_2-2)$；对右边检验，查表求$t$分布的上分位点$t_\alpha(n_1+n_2-2)$；对左边检验，查表求$-t_{\alpha}(n_1+n_2-2)$
4. 根据样本观察值计算$t$的值：$t=\frac{\bar{x}-\bar{y}-\delta}{s_\omega\sqrt{1/m_1+1/n_2}}$
5. 做出判断：对双边检验，当$\mid t\mid\geq t_{\alpha/2}(n_1+n_2-2)$时，拒绝$H_0$，反之接受$H_0$；对右边检验，当$t\geq t_\alpha(n_1+n_2-2)$时，拒绝$H_0$，反之接受$H_0$；对左边检验，当$t\leq -t_{\alpha}(n_1+n_2-2)$时，拒绝$H_0$，反之接受$H_0$。

#### 两个正态总体方差比的假设检验

1. 根据题意，提出如下假设之一：$H_0:\sigma_1^2=\sigma_2^2,H_1:\sigma_1^2\neq\sigma_2^2;\\H_0:\sigma_1^2\leq\sigma_2^2,H_1:\sigma_1^2>\sigma_2^2;\\H_0:\sigma_1^2\geq\sigma_2^2,H_1:\sigma_1^2<\sigma_2^2$.
2. 选取$F$检验统计量：$F=\frac{S_1^2}{S_2^2}$
3. 对给定的$\alpha$，查表求临界点：对双边检验，查表求$F$分布的两个上分位点$F_{1-\alpha/2}(n_1-1,n_2-1)$和$F_{\alpha/2}(n_1-1,n_2-1)$；对右边检验，查表求上分位点$F_\alpha(n_1-1,n_2-1)$；对左边检验，查表求上分位点$F_{1-\alpha}(n_1-1,n_2-1)$
4. 根据样本观察值计算$F$的值：$f=\frac{s_1^2}{s_2^2}$
5. 做出判断：对双边检验，当$f\leq F_{1-\alpha/2}(n_1-1,n_2-1)$或$f\geq F_{\alpha/2}(n_1-1,n_2-1)$时，拒绝$H_0$，反之接受$H_0$，对右边检验，当$f\geq F_\alpha(n_1-1,n_2-1)$时，拒绝$H_0$，反之接受$H_0$；对左边检验，当$f\leq F_{1-\alpha}(n_1-1,n_2-1)$时，拒绝$H_0$，反之接受$H_0$。



|               检验类型                |                             假设                             |                          检验统计量                          |                            拒绝域                            |
| :-----------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 均值检验($\sigma_1^2,\sigma_2^2$已知) | $H_0:\mu_1-\mu_2=\delta,H_1:\mu_1-\mu_2\neq\delta;\\H_0:\mu_1-\mu_2\leq\delta,H_1:\mu_1-\mu_2>\delta;\\H_0:\mu_1-\mu_2\geq\delta,H_1:\mu_1-\mu_2<\delta.$ | $Z=\frac{\bar{X}-\bar{Y}-\delta}{\sqrt{\sigma_1^2/n_1+\sigma_2^2/n_2}}$ | $\mid z\mid\geq z_{\alpha/2}\\z\geq z_\alpha\\z\leq-z_\alpha$ |
| 均值检验($\sigma_1^2=\sigma_2^2$未知) | $H_0:\mu_1-\mu_2=\delta,H_1:\mu_1-\mu_2\neq\delta;\\H_0:\mu_1-\mu_2\leq\delta,H_1:\mu_1-\mu_2>\delta;\\H_0:\mu_1-\mu_2\geq\delta,H_1:\mu_1-\mu_2<\delta.$ | $t=\frac{\bar{X}-\bar{Y}-\delta}{S_{\omega}\sqrt{1/n_1+1/n_2}}\\S_{\omega}^2=\frac{(n_1-1)S_1^2+(n_2-1)S_2^2}{n_1+n_2-2}$ | $\mid t\mid\geq t_{\alpha/2}(n_1+n_2-2)\\t\geq t_\alpha(n_1+n_2-2)\\t\leq -t_{\alpha}(n_1+n_2-2)$ |
|          方差检验($\mu$未知)          | $H_0:\sigma_1^2=\sigma_2^2,H_1:\sigma_1^2\neq\sigma_2^2;\\H_0:\sigma_1^2\leq\sigma_2^2,H_1:\sigma_1^2>\sigma_2^2;\\H_0:\sigma_1^2\geq\sigma_2^2,H_1:\sigma_1^2<\sigma_2^2$ |                   $F=\frac{S_1^2}{S_2^2}$                    | $f\leq F_{1-\alpha/2}(n_1-1,n_2-1)或\\f\geq F_{\alpha/2}(n_1-1,n_2-1)\\f\geq F_\alpha(n_1-1,n_2-1)\\f\leq F_{1-\alpha}(n_1-1,n_2-1)$ |



------

​         