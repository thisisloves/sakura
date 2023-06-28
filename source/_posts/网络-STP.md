---
title: STP
date: 2019-4-5
tags: 网络
author: 映雪
---

终日两相思，为君憔悴尽，百花时。

<!--more-->

### STP（生成树协议）

- 生成树协议（spanning-tree-protocol,stp），就是在具有物理环路的交换机网络上生成没有回路的逻辑网络的方法，生成树协议使用生成树算法，在一个具有冗余路径的容错网络中计算出一个无环路的路径，使一部分端口处于转发状态，另一部分处于阻塞状态（备份状态），从而生成一个稳定的、无环路的生成树网络拓扑，而且一旦发现当前路径故障，生成树协议能立即激活相应的端口，打开备用链路，重新生成 STP 网络拓扑，从而保持网络的正常工作。

### STP 作用

- 通过阻断冗余链路，使一个有回路的桥接网络修剪成一个无回路的树形拓扑结构。

### STP 工作过程

1. 具有最高优先级（优先级 ID 的值最小）的交换机被选为根交换机
2. 在选举出根交换机后，所有的非根交换机会选择到达根交换机的最短路径
3. 选举出根交换机和最短路径后，根端口和指定端口也随之确定
4. 当网络拓扑发生变化时，交换机会自动启用备份链路


### STP 端口状态

1. blocking（阻塞状态) 

- 此时的端口为非指定端口、不会参与数据帧的转发、只会通过接收BPDU来判断根交换机的位置和根ID、以及在STP拓扑收敛结束之后个交换机处于什么状态、默认情况下端口会停留20秒。

2. listening（侦听状态）

- 表明生成树已经根据交换机所接收的BPDU判断出这个端口应该参与数据帧的转发、同时该接口不仅会接收BPDU 数据还会发送自己的BPDU、用来通告邻接交换机此端口会在活动拓扑中参与数据转发的工作，默认情况此端口在这种状态下停留15秒

3. learning（学习状态）

- 此端口参与数据帧的转发，并填写MAC地址表、默认情况下端口在这种状态下停留15秒。

4. forwarding（转发状态）

- 此时端口已经成为活动拓扑的一部分，他会转发数据帧，并同时发送BPDU数据。

5. Disabled（禁用状态）

- 此接口不会参与生成树选举，也不会转发数据帧、直到生成树拓扑发生变化。


### STP 配置


![截屏2022-06-21 下午3.18.39.png](/images/2022/06/21/twRfJiTlrUbjPoG.png)

1. 在交换机内开启STP生成树模式

```js
<Huawei>system-view      进入特权模式
[Huawei]sysname SW1      更改交换机名称为SW1
[SW1]stp mode stp        开启STP生成树功能
 
 
<Huawei>system-view      进入特权模式
[Huawei]sysname SW2      更改交换机名称为SW2
[SW2]stp mode stp        开启STP生成树功能
 
 
<Huawei>system-view       进入特权模式
[Huawei]sysname SW3      更改交换机名称为SW3
[SW3]stp mode stp        开启STP生成树功能
```

2. 指定跟网桥和备份跟网桥

```js
[SW1]stp root primary    指定SW1交换机为根网桥
[SW3]stp root secondary   指定SW3交换机为备份根网桥
```

3. 开启SW3端口开销值计算方法并配置端口路径开销值、实现阻塞端口

```js
[SW3]stp pathcost-standard legacy      开启端口路径开销计算方法为华为计算方法
[SW3]interface GigabitEthernet 0/0/3      进入g0/0/3接口
[SW3-GigabitEthernet0/0/3]stp cost 20000  配置改接口开销值为20000并阻塞此接口。
```

4. 实现STP破除环路故障、将交换机与PC及相连的接口配置为边缘端口

```
[SW2]stp pathcost-standard legacy                  开启路径开销计算方法为华为计算方法
[SW2]interface GigabitEthernet 0/0/2	           进入交换机链接PC机接口
[SW2-GigabitEthernet0/0/2]stp  edged-port enable   配置为边缘接口
[SW2-GigabitEthernet0/0/2]quit                     退出此模式
[SW2]stp bpdu-protection                           开启BPDU保护功能
 
 
[SW3]stp pathcost-standard legacy                  开启路径开销计算方法为华为计算方法
[SW3]interface GigabitEthernet 0/0/1               进入交换机链接PC机接口
[SW3-GigabitEthernet0/0/1]stp edged-port enable    配置为边缘接口
[SW3-GigabitEthernet0/0/1]quit                     退出此模式
[SW3]stp bpdu-protection                           开启BPDU保护功能
```

5. 在所有交换机启用STP功能

```js
[SW1]stp enable 
[SW2]stp enable 
[SW3]stp enable
```