---
title: network
date: 2022-07-08 15:47:02
tags:
- computer science
categories:
- learn in NJUST
---
### 第一章：计算机网络和因特网

* **传输速率**（transmission rate）
* **点对点协议**：PPP
* **分组**（packet）：当一台端系统要向另一台端系统发送数据时，发送端系统将数据分段，并为每段加上首部字节。由此形成的信息包用术语来说称为**分组**
* **因特网服务提供商**（Internet Service Provider，ISP）

<!-- more -->

> 家庭的DSL调制解调器得到数字数据后将其转换成高频音，以通过电话线传输给本地中心局；来自许多家庭的模拟信号在DSLAM处被转换回数字形式

> 物理媒体分成两种类型：**导引型媒体**和**非导引型媒体**
>
> 双绞线连接设备两种方法：**直连线**和**交叉线**
>
> 同轴电缆可分为两种基本类型：**基带同轴电缆**和**宽带同轴电缆**
>
> 一般按传输模式的不同把光纤分为两种：**多模光纤（MMF）**和**单模光纤（SMF）**

* **协议**（protocol）：定义了在两个或多个通信实体之间交换的报文的格式和顺序，以及报文发送和/或接收一条报文或其他事件所采取的动作

> 协议：对等层实体(peer entity)之间在相互通信的过程中，需要遵循的规则的集合，水平（*来自PPT*）

* **频分复用**（Frequency-Division Multiplexing，FDM）
* **时分复用**（Time-Division Multiplexing，TDM）
* **带宽**（band-width）
* **调制**：数字信号转化为模拟信号
* **解调**：模拟信号转化为数字信号
* **静默期**（silent period）
* **节点处理时延**（nodal processing delay）：检查分组首部和决定将该分组导向何处所需要的时间是**处理时延**的一部分
* **排队时延**（queuing delay）：在队列中，当分组在链路上等待传输时，它经受**排队时延**
* **传输时延**（transmission delay）：将所有分组的比特推向链路所需要的时间。在毫秒到微秒量级
* **传播时延**（propagation delay）：从该链路的起点到路由器B传播所需要的时间

$$
d_{nodal}=d_{proc}+d_{queue}+d_{trans}+d_{prop}
$$



* **流量强度**（traffic intensity）
* **丢失**（lost）：由于没有地方存储这个分组，路由器将**丢弃**（drop）该分组，即该分组会**丢失**
* **瞬时吞吐量**（instantaneous throughput）：在任何时间瞬时的**瞬时吞吐量**是主机B接收到该文件的速率（以*bps*计）
* **平均吞吐量**（average throughput）：如果该文件由F比特组成，主机B接收到所有F比特用去T秒，则文件传输的**平均吞吐量**是*F/T bps*
* **瓶颈链路**（bottleneck link）
* **协议栈**（protocol stack）：各层的所有协议被称为**协议栈**
* **应用层**：应用层是网络应用程序及它们的应用层协议存留的地方
* **运输层**：因特网的运输层在应用程序端点之间传送应用层报文
* **网络层**：因特网的网络层负责将称为数据报的网络层分组从一台主机移动到另一台主机
* **链路层**：在每个节点，网络层将数据报下传给链路层，链路层沿着路径将数据报传递给下一个节点。在该下一个节点，链路层将数据报上传给网络层
* **报文**（message）：我们把这种位于应用层的信息分组称为**报文**
* **报文段**（segment）：我们把运输层的分组称为**报文段**
* **数据报**（datagram）：因特网的网络层负责将**数据报**的网络层分组从一台主机移动到另一台主机
* **帧**（frame）：我们把链路层分组称为**帧**
* **OSI参考模型**：应用层、表示层、会话层、运输层、网络层、数据链路层和物理层
* **封装**（encapsulation）
* **运输层报文段**（transport-layer segment）：应用层报文和运输层首部信息一道构成了**运输层报文段**
* **网络层数据报**（network-layer datagram）：运输层则向网络层传递运输层报文段，网络层增加了如源和目的端系统地址等网络层首部信息，生成了**网络层数据报**
* **链路层帧**（link-layer frame）：网路层数据报接下来被传递给链路层，链路层增加它自己的链路层首部信息并生成**链路层帧**
* **有效载荷字段**（payload field）：有效载荷通常是来自上一层的分组
* **僵尸网络**（botnet）
* **拒绝服务攻击**（Denial-of-Service attack，DOS attack）
* **IP哄骗**（IP spoofing）



### 第二章：应用层

> 两种网络应用程序体系结构：**客户-服务器体系结构给**或**对等（P2P）体系结构**

* **进程**

> 在一对进程之间的通信会话场景中，发起通信的进程被标识为**客户**，在会话开始时等待联系的进程是**服务器**

* **套接字**（socket）：进程通过一个称为**套接字**的软件接口向网络发送报文和从网络接受报文。**套接字**也被称为应用程序和网络之间的**应用程序编程接口**（Application Programming Interface）
* **IP地址**（IP address）：主机由其**IP地址**标识
* **用户代理**（user agent）
* **端口号**（port number）：用于标识运行在接受主机上的接收进程（更具体地说，接收套接字）
* **可靠数据传输**（reliable data transfer）
* **容忍丢失的应用**（loss-tolerant application）
* **吞吐量**（throughput）
* **定时**（timing）
* **流媒体**（stcp）

> 大体上对应用程序服务要求进行分类：可靠数据传输、吞吐量、定时和安全性

**应用层协议**（application-layer protocol）

1. 交换的报文类型
2. 各种报文类型的语法
3. 字段的语义
4. 确定一个进程何时以及如何发送报文，对报文进行相应的规则



> UDP是一种不提供不必要服务的轻量级运输协议，它仅提供最小服务。UDP协议提供一种不可靠数据传送服务。UDP没有包括拥塞控制机制，所以UDP的发送端可以用它选定的任何速率向其下层注入数据。

* **超文本传输协议**（HyperText Transfer Protocol， HTTP）

> HTTP是一个**无状态协议**（stateless protocol）

* **非持续连接**（non-persistent connection）
* **持续连接**（persistent connection）
* **采用非持续连接的HTTP**：HTTP 1.0
* **采用持续连接的HTTP**：HTTP 1.1
* **cookie**：某些网站为了辨别用户身份、进行session跟踪而储存在用户本地终端上的数据（通常经过加密）（*来自作业*）

> **Web缓存器**也叫**代理服务器**，它是能够代表初始Web服务器来满足HTTP请求的网络实体。

> **条件GET**允许缓存器证实它的对象是最新的。

> 电子邮件系统总体结构的三个组成部分：**用户代理**、邮件服务器和**简单的邮件传输协议**（SMTP）

* **简单的邮件传输协议**（Simple Mail Transfer Protocol，SMTP）：SMTP用于从发送方的邮件服务器发送报文到接收方的邮件服务器
* **邮件访问协议**：用于将邮件从邮件服务器传送到接收方的用户代理

* **第三版的邮局协议**（Post Office Protocol-Version 3，POP3）
* **因特网邮件访问协议**（Internet Mail Access Protocol， IMAP）
* **因特网的目录服务**（domain name system，DNS）



DNS是

1. 一个由分层的DNS服务器实现的分布式数据库
2. 一个使得主机能够查询分布式数据库的应用层协议



* **分布式、层次数据库**（distributed，hierarchical database）
* **分布式拒绝服务**（DDoS）
* **内容分发网**（content distribution network，CDN）
* **集群选择策略**（cluster selection strategy）：任何CDN部署，其核心是**集群选择策略**



### 第三章：运输层

* **用户数据报协议**（User Datagram Protocol，UDP）

1. 关于发送什么数据以及何时发送的应用层控制更为精细
2. 无须连接建立
3. 无连接状态
4. 分组首部开销小



* **传输控制协议**（TCP，Transmission Control Protocol）
* **不可靠服务**（unreliable service）
* **运输层的多路复用**（transport-layer multiplexing）：在源主机从不同套接字中收集数据块，并为每个数据块封装上首部信息从而生成报文段，然后将报文段传递到网络层，所有这些工作称**多路复用**
* **多路分解**（demultiplexing）：在接收端，运输层检查这些字段，标识出接收套接字，进而将报文段定向到该套接字。将运输层报文段中的数据交付到正确的套接字的工作称为**多路分解**
* **可靠数据传输**（reliable data transfer）
* **拥塞控制**（congestion control）
* **单向数据传输**（unidirectional data transfer）
* **双向数据传输**（bidirectional data transfer）
* **自动重传请求**（Automatic Repeat reQuest，ARQ）
* **回退N步**（Go-Back-N，GBN）
* **选择重传**（Selective Repeat，SR）
* **最大报文段长度**（Maximum Segment Size，MSS）：MSS通常根据最初确定的由本地发送主机发送的最大链路层帧长度（即所谓的**最大传输单元**（Maximum Transmission Unit，MTU）来设置

> TCP在IP不可靠的尽力而为服务之上创建了一种**可靠数据传输服务**（reliable data transfer service）

* **流量控制服务**（flow-control service）：流量控制是一个速度匹配服务，即发送方的发送速率与接收方应用程序的读取速率相匹配（ 控制发送进程，使接收进程来得及接收）
* **拥塞控制**（congestion control）：TCP发送方也可能因为IP网络的拥塞而被遏制；这种形式的发送方的控制被称为**拥塞控制**（控制发送进程，使网络来得及传送）

> TCP通过让*发送方*维护一个称为**接收窗口**（receive window）的变量来提供流量控制

 

TCP拥塞控制算法包括三个主要部分

1. 慢启动
2. 拥塞避免
3. 快速回复



### 第四章：网络层：数据平面

自治系统内部协议：*OSPF*、*RIP（Routing Information Protocol）*

自治系统外部协议：*BGP*

* **转发**（forwarding）：指将分组从一个输入链路接口转移到适当的输出链路接口的路由器本地动作
* **路由选择**（routing）：指确定分组从源到目的地所采取的端到端路径的网络范围处理过程
* **软件定义网络**（Software-Defined Networking，SDN）
* **链路层交换机**（link-layer switch）：基于链路层帧中的字段值做出转发决定
* **路由器**（router）：基于网络层数据报中的首部字段值做出转发决定

> 路由器的四个组件：输入端口、交换结构、输出端口和路由选择处理器

* **输入端口**：在路由器中执行终结入物理链路的物理层功能
* **交换结构**：将路由器的输入端口连接到它的输出端口
* **输出端口**：存储从交换结构接收的分组，并通过执行必要的链路层和物理层功能在输出链路上传输这些分组
* **路由选择处理器**：执行控制平面功能

> 三种交换结构：经内存交换、经总线交换、经互联网络交换

* **转发表**（forwarding table）：转发表是由路由选择处理器计算和更新的，或者转发表接收来自远程SDN控制器的内容
* **最长前缀匹配规则**（longest prefix matching rule）：在最长匹配转发表中寻找最长的匹配项，并向最长前缀匹配相关联的链路接口转发分组
* **丢包**（packet loss）：随着路由器输入或输出端口排队的队列的增长，路由器的缓存空间最终将会耗尽，并且当无内存可用于存储到达的分组时将会出现丢包
* **线路前部**（Head-Of-the-Line，HOL）**阻塞**
* **主动队列管理**（Active Queue Management，AQM）算法
* **随机早期检测**（Random Early Detection，RED）算法
* **分组调度**（packet scheduler）
* **先进先出**（First-In-First-Out，FIFO）：也称**先来先服务**（FCFS）
* **优先权排队**（priority queuing）
* **非抢占式优先权排队**（non-preemptive priority queuing）
* **循环排队规则**（round robin queuing discipline）
* **保持工作排队**（work-conserving queuing）
* **加权公平排队**（Weighted Fair Queuing，WFQ）

> IP分组交付服务主要特点：不可靠、尽力而为、无连接

IPv4数据包中的关键字段如下：

1. 版本
2. 首部长度
3. 服务类型
4. 数据报长度
5. 标识、标志、片偏移
6. 寿命
7. 协议
8. 首部检验和
9. 源和目的IP地址
10. 选项
11. 数据（有效载荷）



> IPv4地址通常按所谓**点分十进制记法**（dotted-decimal notation）书写

* **子网掩码**（network mask）：指示32比特中的最左侧24比特定义了子网地址
* **无类别域间路由选择**（Classless Interdomain Routing，CIDR）：因特网的地址分配策略被称为**无类别域间路由选择**
* **动态主机配置协议**（Dynamic Host Configuration，DHCP）：允许主机自动获取（被分配）一个IP地址，被称为**即插即用协议**或**零配置协议**



DHCP协议的4个步骤：

1. DHCP服务器发现
2. DHCP服务器提供
3. DHCP请求
4. DHCP ACK



* **网络地址转换**（Network Address Translation，NAT）

> 路由器怎样知道它应将某个分组转发给哪个内部主机呢？
>
> 技巧就是使用NAT路由器上的一张**NAT转化表**（NAT translation table），并且在表项中包含了端口号及其IP地址



IPv6中定义的字段：

1. 版本
2. 流量类型
3. 流标签
4. 有效载荷长度
5. 下一个首部
6. 跳限制
7. 源地址和目的地址
8. 数据



* **流表**（flow table）：匹配加动作转发表在OpenFlow中称为**流表**



### 第五章：网络层：控制平面

* **互联网控制报文协议**（ICMP）：被主机和路由器用来彼此沟通网络层的信息
* **简单网络管理协议**（SNMP）：SNMP是要给应用层协议，用于在管理服务器和代表管理服务器执行的代理之间传递网络管理控制和信息报文
* **链路状态**（Link State，LS）**算法**：具有全局状态信息的算法常被称作**链路状态算法**
* **距离向量**（Distance-Vector）**算法**
* **路由选择表**（routing table）
*  **毒性逆转**（poisoned reverse）
* **自治系统**（Autonomous System，AS）：聚合路由器为一个区域
* **自治系统内部路由选择协议**（intra-autonomous system routing protocol）：在一个自治系统内运行的路由选择算法
* **开放最短路优先**（OSPF）
* **自治系统间路由选择协议**（inter-autonomous system routing protocol）
* **边界网关协议**（Broder Gateway Protocol，BGP）




BGP为每台路由器提供了一种完成以下人物的手段：

1. 从邻居AS获得前缀的可达性信息
2. 确定到该前缀的“最好的”路由



* **热土豆路由选择**（hot potato routing）
* **多宿接入ISP**（multi-homed stub network）



SDN体系结构具有4个关键特征：

1. 基于流的转发
2. 数据平面和控制平面分离
3. 网络控制功能
4. 可编程的网络

> OpenFlow协议运行在SDN控制器和SDN控制的交换器或其他实现OpenFlow API的设备之间。



### 第六章：链路层和局域网

* **成帧**（framing）：在每个网络层数据报经链路传送之前，几乎所有的链路层协议都要将其用链路层帧封装起来
* **链路接入**：**媒体访问控制**（Medium Access Control，MAC）协议规定了帧在链路上传输的规则
* **可靠交付**：当链路层协议提供可靠交付服务时，它保证无差错地经链路层移动每个网络层数据报
* **差错检测和纠正**：通过让发送节点在帧中包括差错检测比特，让接收节点进行差错检查，以此来完成这项工作
* **网络适配器**（network adapter）
* **网络接口卡**（Network Interface Card，NIC）

> 在发送端，控制器取得了由协议栈较高层生成并存储在主机内存中的数据报，在链路层帧中封装该数据报，然后遵循链路接入协议将该帧传进通信链路中。在接收端，控制器接收了整个帧，抽取出网络层数据报。

* **奇偶校验位**（parity bit）
* **前向纠错**（Forward Error Correction，FEC）：接收方检测和纠正差错的能力被称为前向纠错
* **循环冗余检测**（Cyclic Redundancy Check，CRC）**编码**：现今的计算机网络中广泛应用的差错检测技术基于**循环冗余检测编码**。CRC编码也称为**多项式编码**（polynomial code）。
* **点对点链路**：由链路一端的单个发送方和链路另一端的单个接收方组成
* **广播链路**：能够让多个发送和接受节点都连接到相同的、单一的、共享的广播信道中

> 因为所有的节点都能够传输帧，所以多个节点可能会同时传输帧。当发生这种情况时，所有节点同时接到多个帧；这就是说，传输的帧在所有的接收方处**碰撞**。

> 任何多路访问协议都可以分为3种类型之一：信道划分协议、随机接入协议、轮流协议	

* **信道划分协议**（channel partitioning protocol）
* **随机接入协议**（random access protocol）
* **轮流协议**（taking-turns protocol）
* **时间帧**（time frame）
* **时隙**（slot）
* **码分多址**（Code Division Multiple Access，CDMA）
* **时隙ALOHA**：最简单的随机接入协议之一，*效率最多为37%*
* **ALOHA**：第一个ALOHA协议实际上是一个非时隙、完全分散的协议，*效率最多为18%*
* **载波侦听多路访问**（CSMA）
* **具有碰撞检测的CSMA**（CSMA with Collision Detection，CSMA/CD）
* **信道传播时延**（channel propagation delay）：信号从一个节点传播到另一个节点所花费的时间
* **二进制指数后退**（binary exponential backoff）算法
* **轮询协议**（polling protocol）：轮询协议要求这些节点之一要被指定为主节点。主节点以循环的方式轮询每个节点。特别是，主节点首先向节点1发送要给报文，告诉它能够传输的最多数量。在节点1传输了某些帧后，主节点告诉节点2它能够传输的帧的最多数量。上述过程以这种方式继续进行，主节点以循环的方式轮询了每个节点
* **令牌传递协议**（token-passing protocol）：在这种协议中没有主节点。一个称为令牌的小的特殊帧在节点之间以某种固定的次序进行交换
* **电缆调制解调器端接系统**（Cable Modem Termination System，CMTS）
* **数据经电缆服务接口**（Data-Over-Cable Service Interface，CMTS）规范（DOCSIS）
* **地址解析协议**（Address Resolution protocol，ARP）



以太网帧的6个字段：

1. 数据字段（46~1500字节）：承载了IP数据报
2. 目的地址（6字节）：包含目的适配器的MAC地址
3. 源地址（6字节）：包含了传输该帧到局域网上的适配器的MAC地址
4. 类型字段（2字节）：允许以太网复用多种网络层协议
5. CRC（4字节）
6. 前同步码（8字节）



* **转发器**（repeater）
* **过滤**（filtering）：决定一个帧应该转发到某个接口还是应当将其丢弃的交换机功能
* **转发**（forwarding）：决定一个帧应该被导向哪个接口，并把该帧移动到那些接口的交换机功能
* **交换机表**（switch table）：该交换机表包含某局域网上某些主机和路由器但不必是全部的表项



交换机的性质：

1. 消除碰撞
2. 异质的链路
3. 管理



交换机表的一个表项包含：

1. 一个MAC地址
2. 通向该MAC地址的交换机接口
3. 表项放置在表中的时间

> 局域网中常用的拓扑结构主要有**总线型拓扑**、**星形拓扑**、**环形拓扑**、**树形拓扑**（由总线型演变而来）以及它们的混合型。

> MAC地址前24位为厂商号，后24位为编号

* **虚拟局域网**（Virtual Local Network，VLAN）
* **标签协议标识符**（Tag Protocol Identifier，TPID）
* **多协议标签交换**（Multiprotocol Label Switching，MPLS）
* **虚拟专用网**（Virtual Private Network，VPN）
* **全连接拓扑**（fully connected topology）
* **模块化数据中心**（Modular Data Center，MDC）
* **自组织网络**（ad hoc network）

> 基带传输：信号源产生的原始电信号称为基带信号，将数字数据0、1直接用两种不同的电压表示，然后送到线路上去传输。
>
> 宽带传输：将基带信号进行调制后形成模拟信号，然后采用频分复用技术实现宽带传输。

> 常用以太网技术：*10BASE-2*、*100BASE-T*、*1000BASE-LX*、*10GBASE-T*

> 直通转发：交换机一旦解读到数据包目的地址，就开始向目的端口发送数据包
>
> 存储转发：存储转发技术要求交换机在接收到全部数据包后再决定如何转发

### 第七章：无线网络和移动网络

有线链路和无线链路间的许多重要区别：

1. 递减的信号强度
2. 来自其他源的干扰
3. 多径传播

> 无线网络四个组成要素：无线主机、无线链路、基站、网络基础设施

> 无线网络两种通信模式：基础设施模式、自组织ad hoc模式

> 无线网络四种类型：单跳，基于基础设施、单跳，无基础设施、多跳，基于基础设施、多跳，无基础设施

* **信噪比**（Signal-to-Noise Ratio，SNR）
* **比特差错率**（BER）
* **基本服务集**（Basic Service Set，BSS）
* **接入点**（Access Point，AP）
* **信标帧**（beacon frame）
* **带碰撞避免的CSMA**（CSMA with collision avoidance，CSMA/CA）

> 由于*802.11*无线局域网不使用碰撞检测，一旦站点开始发送一个帧，它就完全地发送该帧；也就是说，一旦站点开始发送，就不会返回。碰撞存在时仍发送整个数据帧将严重降低多路访问协议的性能。为了降低碰撞的可能性，802.11采用了集中碰撞避免技术。

* **短帧间间隔**（Short Inter-Frame Spacing ，SIFS）
* **分布式帧间间隔**（Distributed Inter-Frame Space，DIFS）
* **无线个人域网络**（Wireless Personal Area Network，WPAN）
* **跳频扩展频谱**（Frequency-Hopping Spread Spectrum，FHSS）
* **皮可网**（piconet）
* **直接序列宽带CDMA**（Direct Sequence Wideband CDMA，DS-WCDMA）
* **正交频分复用**（Orthogonal Frequency Division Multiplexing，OFDM）
* **转交地址**（Care-Of Address，COA）

> 术语**蜂窝**（cellular）是指这样的事实，即由一个蜂窝网覆盖的区域被分成许多称作**小区**（cell）的地理覆盖范围。每个小区包含一个**收发基站**（Base Transceiver Station，BTS），负责向位于其小区内的移动站点发送或接收信号

2G：语音和电话网连接

3G：将因特网扩展到蜂窝用户

4G：一个全IP核心网

---

A类：(1.0.0.0-126.0.0.0)（默认子网掩码：255.0.0.0或 0xFF000000）第一个字节为网络号，后三个字节为主机号。该类IP地址的最前面为“0”，所以地址的网络号取值于1~126之间。一般用于大型网络。

B类：(128.0.0.0-191.255.0.0)（默认子网掩码：255.255.0.0或0xFFFF0000）前两个字节为网络号，后两个字节为主机号。该类IP地址的最前面为“10”，所以地址的网络号取值于128~191之间。一般用于中等规模网络。

C类：(192.0.0.0-223.255.255.0)（子网掩码：255.255.255.0或 0xFFFFFF00）前三个字节为网络号，最后一个字节为主机号。该类IP地址的最前面为“110”，所以地址的网络号取值于192~223之间。一般用于小型网络。

D类：是多播地址。该类IP地址的最前面为“1110”，所以地址的网络号取值于224~239之间。一般用于多路广播用户[1] 。

E类：是保留地址。该类IP地址的最前面为“1111”，所以地址的网络号取值于240~255之间。

在IP地址3种主要类型里，各保留了3个区域作为私有地址，其地址范围如下： 
A类地址：10.0.0.0～10.255.255.255 
B类地址：172.16.0.0～172.31.255.255 
C类地址：192.168.0.0～192.168.255.255

回送地址：127.0.0.1。 也是本机地址，等效于localhost或本机IP。一般用于测试使用。例如：ping 127.0.0.1来测试本机TCP/IP是否正常。

---

