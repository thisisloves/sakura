---
title: OSPF
date: 2020-4-7
tags: 网络
author: 映雪
---

终是谁使弦断，花落肩头，恍惚迷离。

<!--more-->

### OSPF（开放最短路径优先）

- OSPF（Open Shortest Path First）路由协议是一种典型的**链路状态**（Link-state）的路由协议，一般用于同一个路由域内。在这里，路由域是指一个**自治系统**（Autonomous System），即 AS，它是指一组通过统一的路由政策或路由协议互相交换路由信息的网络。在这个 AS 中，所有的 OSPF 路由器都维护一个相同的描述这个 AS 结构的数据库，该数据库中存放的是路由域中相应链路的状态信息，OSPF 路由器正是通过这个数据库计算出其 OSPF 路由表的。
- 作为一种链路状态的路由协议，OSPF 将**链路状态广播数据包 LSA**（Link State Advertisement）传送给在某一区域内的所有路由器，这一点与距离矢量路由协议不同。运行距离矢量路由协议的路由器是将部分或全部的路由表传递给与其相邻的路由器。

### OSPF 特性

1. 适应范围广：支持大规模网络，最多可支持几百台路由器。
2. 支持掩码：由于 OSPF 报文中携带掩码信息，所以 OSPF 协议不受自然掩码的限制，对 VLSM 提供很好的支持。
3. 快速收敛：在网络的拓扑结构发生变化后立即发送更新报文，使这一变化在自治系统中同步。
4. 无自环：由于 OSPF 根据收集到的链路状态用最短路径树算法计算路由，从算法本身保证了不会生成自环路由。
5. 区域划分：允许自治系统的网络被划分成区域来管理，区域间传送的路由信息被进一步抽象，从而减少了占用的网络带宽。
6. 等价路由：支持到同一目的地址的多条等价路由。
7. 路由分级：使用 4 类不同的路由，按优先顺序来说分别是：区域内路由、区域间路由、第一类外部路由、第二类外部路由。
8. 支持验证：支持基于区域和接口的报文验证，以保证报文交互的安全性。
9. 组播发送：在某些类型的链路上以组播地址发送协议报文，减少对其他设备的干扰。

### OSPF 工作过程

1. 每台路由器学习激活的直接相连的网络。
2. 每台路由器和直接相连的路由器互交，发送 Hello 报文，建立邻居关系。
3. 每台路由器构建包含直接相连的链路状态的 LSA（Link-State Advertisement，链路状态通告 LSA）。链路状态通告(LSA)中记录了所有相关的路由器，包括邻路由器的标识、链路类型、带宽等。
4. 每台路由器泛洪链路状态通告（LSA）给所有的邻路由器，并且自己也在本地储存邻路由发过来的 LSA，然后再将收到的 LSA 泛洪给自己的所有邻居，直到在同一区域中的所有路由器收到了所有的 LSA。每台路由器在本地数据库中保存所有收到的 LSA 副本，这个数据库被称作"链路状态数据库（LSDB，Link-State Database）"
5. 每台路由器基于本地的"链路状态数据库(LSDB)"执行"最短路径优先（SPF）"算法，并以本路由器为根，生成一个 SPF 树，基于这个 SPF 树计算去往每个网络的最短路径，也就生成了路由表。

- 五步总结：建立邻居关系——>相互转发 LSA——构建链路状态数据——执行 SPF 算法——生成路由条目

- 邻居表：记录所有建立了邻居关系的路由器，包括相关描述和邻居状态。会定期的相互发送 hello 报文来维护，若在一定的周期内没有收到领居回应的 hello 报文，则认为邻居路由器失效，将它从邻居表中删除。
- 链路状态数据库表（LSDB）：此表里包含了网络拓扑中链路状态的通告。每台路由器在同一个区域内 LSDB 表一样。
- 路由表：在获得完整 LSDB 表后，进行 SPF 算法，形成最优路由加入路由表。

### OSPF 分组

- OSPF 协议依靠五种不同类型的分组来建立邻接关系和交换路由信息，即问候分组、数据库描述分组、链路状态请求分组、链路状态更新分组和链路状态确认分组。

1. 问候(Hello)分组

- OSPF 使用 Hello 分组建立和维护邻接关系。在一个路由器能够给其他路由器分发它的邻居信息前，必须先问候它的邻居们。

2. 数据库描述(Data base Description，DBD)分组

- DBD 分组不包含完整的“链路状态数据库”信息，只包含数据库中每个条目的概要。当一个路由器首次连入网络，或者刚刚从故障中恢复时，它需要完整的“链路状态数据库”信息。此时，该路由器首先通过 hello 分组与邻居们建立双向通信关系，然后将会收到每个邻居反馈的 DBD 分组。新连入的这个路由器会检查所有概要，然后发送一个或多个链路状态请求分组，取回完整的条目信息。

3. 链路状态请求(Link State Request，LSR)分组

- LSR 分组用来请求邻居发送其链路状态数据库中某些条目的详细信息。当一个路由器与邻居交换了数据库描述分组后，如果发现它的链路状态数据库缺少某些条目或某些条目已过期，就使用 LSR 分组来取得邻居链路状态数据库中较新的部分。

4. 链路状态更新(Link State Update，LSU)分组

- LSU 分组被用来应答链路状态请求分组，也可以在链路状态发生变化时实现洪泛(flooding)。在网络运行过程中，只要一个路由器的链路状态发生变化，该路由器就要使用 LSU，用洪泛法向全网更新链路状态。

5. 链路状态确认(Link State Acknowledgment，LSAck)分组

- LSAck 分组被用来应答链路状态更新分组，对其进行确认，从而使得链路状态更新分组采用的洪泛法变得可靠。

### OPSF 协议优缺点

- 优点

1. 运行链路状态路由协议的路由器通过 LSA 的交换，最后独立的计算出到每个网络的最短路径，相对距离矢量路由具有更强的全局观
2. 收到邻居的 LSA 后立即泛洪，并且本路由再执行 SPF 算法，比距离矢量路由有更高的收敛速度
3. 当检测到拓扑发生变化时立即发送更新
4. 多区域设计，可以将一些问题限制在较小的区域中

- 缺点

1. 内存需求高，需要更强的 CPU 的支持；
2. 在网络初始化时，大量链路状态包泛洪，会影响网络的可用带宽；

### 三层交换机 OSPF 配置

![截屏2022-06-24 下午2.53.10.png](/images/2022/06/24/HeLpViWUS5oFr3b.png)

1. 接入层交换机配置

```
[as-sw1]vlan 10
[as-sw1]int g0/0/1
[as-sw1-GigabitEthernet0/0/1]port link-type access // access 默认模式 多用于连接客户端 
[as-sw1-GigabitEthernet0/0/1]port default vlan 10
[as-sw1-GigabitEthernet0/0/1] q
[as-sw1]int g0/0/2
[as-sw1-GigabitEthernet0/0/2]port link-type trunk  // trunk多用于交换机设备互联vlan 通信
[as-sw1-GigabitEthernet0/0/2]port trunk allow-pass vlan 10
[as-sw1-GigabitEthernet0/0/2] q

[as-sw2]vlan 20
[as-sw2]int g0/0/1
[as-sw2-GigabitEthernet0/0/1]port link-type access
[as-sw2-GigabitEthernet0/0/1]port default vlan 20
[as-sw2-GigabitEthernet0/0/1] q
[as-sw2]int g0/0/2
[as-sw2-GigabitEthernet0/0/2]port link-type trunk
[as-sw2-GigabitEthernet0/0/2]port trunk allow-pass vlan 20
[as-sw2-GigabitEthernet0/0/2] q

```

2. 核心交换机配置

```
[core-sw1]vlan batch 10 12
[core-sw1]int g0/0/2
[core-sw1-GigabitEthernet0/0/2]port link-type trunk
[core-sw1-GigabitEthernet0/0/2]port trunk allow-pass vlan 10
[core-sw1-GigabitEthernet0/0/2]q
[core-sw1]int g0/0/24
[core-sw1-GigabitEthernet0/0/24]port link-type trunk
[core-sw1-GigabitEthernet0/0/24]port trunk allow-pass vlan 12
[core-sw1-GigabitEthernet0/0/24]undo prot trunk allow-pass vlan 1
[core-sw1-GigabitEthernet0/0/24]q
// 配置VLANIF 接口的 ip 地址
[core-sw1]int vlanif 10
[core-sw1-Vlanif10]ip address 192.168.10.254 24
[core-sw1-Vlanif10]q
[core-sw1]int vlanif 12
[core-sw1-Vlanif12]ip address 10.1.12.1 24
[core-sw1-Vlanif10]q
[core-sw1]int loopback0
[core-sw1-LoopBack0]ip address 1.1.1.1 32
[core-sw1-LoopBack0]q
// 配置 OSPF 路由
[core-sw1]ospf 1 route-id 1.1.1.1
[core-sw1-ospf-1]area 0
[core-sw1-ospf-1-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[core-sw1-ospf-1-area-0.0.0.0]network 10.1.12.0 0.0.0.255
[core-sw1-ospf-1-area-0.0.0.0]q
[core-sw1-ospf-1]q


[core-sw2]vlan batch 20 12
[core-sw2]int g0/0/2
[core-sw2-GigabitEthernet0/0/2]port link-type trunk
[core-sw2-GigabitEthernet0/0/2]port trunk allow-pass vlan 20
[core-sw2-GigabitEthernet0/0/2]q
[core-sw2]int g0/0/24
[core-sw2-GigabitEthernet0/0/24]port link-type trunk
[core-sw2-GigabitEthernet0/0/24]port trunk allow-pass vlan 12
[core-sw2-GigabitEthernet0/0/24]q
// 配置VLANIF 接口的 ip 地址
[core-sw2]int vlanif 20
[core-sw2-Vlanif20]ip address 192.168.20.254 24
[core-sw2-Vlanif20]q
[core-sw2]int vlanif 12
[core-sw2-Vlanif12]ip address 10.1.12.2 24
[core-sw2-Vlanif12]q
[core-sw2]int loopback0
[core-sw2-LoopBack0]ip address 2.2.2.2 32
[core-sw2-LoopBack0]q
// 配置 OSPF 路由
[core-sw2]ospf 1 route-id 2.2.2.2
[core-sw2-ospf-1]area 0
[core-sw2-ospf-1-area-0.0.0.0]network 192.168.20.0 0.0.0.255
[core-sw2-ospf-1-area-0.0.0.0]network 10.1.12.0 0.0.0.255
[core-sw2-ospf-1-area-0.0.0.0]q
[core-sw2-ospf-1]q
```

3. PC1 与 PC2 此时已经能够实现互通
