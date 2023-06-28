---
title: ISIS
date: 2020-5-05
tags: 网络
author: 映雪
---

一生一世，如梦初醒。逐梦令，浮生半醒。泪满襟，笑缘尽。

<!--more-->

### ISIS(中间系统到中间系统)

- ISIS（Intermediate System-to-Intermediate System）是一个分级的链接状态路由协议。基于 DECnet PhaseV 路由算法，实际上与 OSPF 非常相似，它也使用 Hello 协议寻找毗邻节点，使用一个传播协议发送链接信息。ISIS 可以在不同的子网上操作，包括广播型的 LAN、WAN 和点到点链路。

![截屏2022-06-28 上午10.40.49.png](/images/2022/06/28/hZ8tszqDkaLuOm3.png)

### ISIS 特征

1. 维护一个链路状态数据库，并使用 SPF 算法来计算最佳路径。
2. 用 Hello 数据包建立和维护邻居关系。
3. 为了支持大规模的路由网络，IS-IS 在自治系统内采用骨干区域与非骨干区域两级的分层结构。
4. 在区域之间可以使用路由汇总来减少路由器的负担。
5. 支持 VLSM 和 CIDR，可以基于接口、区域和路由域进行验证，验证方法支持明文验证、MD5 验证和 Keychain 验证。
6. IS-IS 只支持广播和点到点两种网络类型。在广播网络类型中通过选举指定 IS（Designated Intermediate System，DIS）来管理和控制网络上的泛洪扩散。
7. IS-IS 路由优先级为 15，支持宽度量（Wide Metric）和窄度量（Narrow Metric）。IS-IS 路由度量的类型包括默认度量、延迟度量、开销度量和差错度量。默认情况下 IS-IS 采用默认度量，接口的链路开销为 10。
8. 收敛快速，适合大型网络。

### ISIS 工作过程

1. 建立邻居关系
2. 同步 LSDB 数据库
3. 执行 SPF 算法计算路由

### ISIS 拓扑结构

1. IS-IS 在自治系统内采用骨干区域与非骨干区域两级的分层结构。
2. Level-1 路由器部署在非骨干区域，Level-2 路由器和 Level-1-2 路由器部署在骨干区域。
3. 每一个非骨干区域都通过 Level-1-2 路由器与骨干区域相连。
4. 所有物理连续的 Level-1-2 和 Level-2 路由器构成了 IS-IS 的骨干区域。

### ISIS 的路由器种类和功能

- 两种路由器种类

1. L1 负责在同一个区域内传播链路状态信息（类似 OSPF 中的 1 类和 2 类）
2. L2 负责在不同的区域内相互传播链路状态信息（类似 OSPF 的 3 类）

- 三种路由器的功能

1. L1 能获取区域内的路径信息，
2. L2 能获取区域间的路径信息
3. L1-2:能同时获取区域内和区域间路径

### ISIS 与 OSPF 比较

- 相同点

1. 都是应用广泛的 IGP，都是链路状态协议。
2. 都支持 IP 环境。
3. 都采用分层设计和分区域设计。
4. 都通过 Hello 协议发现邻居，形成邻接关系。
5. 在多路访问网络上选举 DR/DIS。
6. 都遵循基本的链路状态数据库同步方法。
7. 都使用 SPF 算法计算最佳路由。
8. 无环路，收敛快。
9. 都支持大规模网络应用。

- 不同点

1. IS-IS 支持 CLNP 环境和 IP 环境，OSPF 仅支持 IP 环境。
2. IS-IS 只支持点到点和广播网络类型,OSPF 支持点到点、广播、点到多点、NBMA 网络类型。
3. 报文封装方式不同，IS-IS 报文封装在数据链路层帧中，OSPF 封装在 IP 包中。
4. OSPF 基于接口划分区域,IS-IS 基于路由器划分区域。
5. 建立邻接关系的条件不同, OSPF 邻居关系建立比 IS-IS 复杂。
6. 点到点链路上 OSPF 的邻接关系形成比 IS-IS 要可靠。
7. IS-IS 工作在二层数据链路层，OSPF 在三层。

### ISIS 配置

![截屏2022-06-29 下午5.11.25.png](/images/2022/06/29/zEYS5N2ycWUZ6kP.png)

1. 在特权模式中进入 IS-IS 配置视图，指定路由器的级别，并配置路由器的 network-entity。
2. 进入相应接口，配置 IP 地址。

- R1 配置

```
isis 1
 is-level level-2
 network-entity 49.0000.0000.0000.0001.00
#
interface GigabitEthernet0/0/0
 ip address 10.1.14.1 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/1
 ip address 10.1.126.1 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/2
 ip address 10.1.13.1 255.255.255.0
 isis enable 1
#
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255
 isis enable 1
```

- R2 配置

```
isis 1
 is-level level-2
 network-entity 49.0002.0000.0000.0002.00
#
interface Serial4/0/0
 link-protocol ppp
 ip address 10.1.26.2 255.255.255.0
#
interface GigabitEthernet0/0/0
 ip address 10.1.126.2 255.255.255.0
 isis enable 1
#
interface LoopBack0
 ip address 2.2.2.2 255.255.255.255
 isis enable 1
```

- R3 配置

```
isis 1
 network-entity 49.0001.0000.0000.0003.00
#
interface GigabitEthernet0/0/0
 ip address 10.1.13.3 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/1
 ip address 10.1.35.3 255.255.255.0
 isis enable 1
#
interface LoopBack0
 ip address 3.3.3.3 255.255.255.255
 isis enable 1
```

- R4 配置

```
isis 1
 network-entity 49.0001.0000.0000.0004.00
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.1.14.4 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/1
 ip address 10.1.45.4 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/2
#
interface NULL0
#
interface LoopBack0
 ip address 4.4.4.4 255.255.255.255
 isis enable 1

```

- R5 配置

```
isis 1
 is-level level-1
 network-entity 49.0001.0000.0000.0005.00
#
interface GigabitEthernet0/0/0
 ip address 10.1.45.5 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/1
 ip address 10.1.35.5 255.255.255.0
 isis enable 1
#
interface LoopBack0
 ip address 5.5.5.5 255.255.255.255
 isis enable 1
```

- R6 配置

```
isis 1
 is-level level-2
 network-entity 49.0002.0000.0000.0006.00
#
interface Serial4/0/0
 link-protocol ppp
 ip address 10.1.26.6 255.255.255.0
 isis enable 1
#
interface GigabitEthernet0/0/0
 ip address 10.1.126.6 255.255.255.0
 isis enable 1
#
interface LoopBack0
 ip address 6.6.6.6 255.255.255.255
 isis enable 1
```
