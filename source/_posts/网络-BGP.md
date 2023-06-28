---
title: BGP
date: 2020-6-15
tags: 网络
author: 映雪
---

欲寄彩笺兼尺素，山长水阔知何处。

<!--more-->

### BGP（边界网关协议）

- BGP (Border Gateway Protocol)是一种**外部网关协议**（EGP），与 OSPF、RIP 、 IGRP 、EIGRP 等**内部网关协议**（IGP）不同，其着眼点不在于发现和计算路由，而在于控制路由的传播和选择最佳路由。
- BGP 主要用于与其他 BGP 线路建立网络连接、相互交换包括 AS 在内的信息。这种信息对于构成 AS 互联的拓扑图、清洗路由环路同时施加决策给 AS 有着极大的成效。

![截屏2022-06-29 下午5.35.41.png](/images/2022/06/29/LVRWrIc9noYlHK8.png)

### BGP 特点

1. BGP 使用 TCP 作为其传输层协议（监听端口号为 179），提高了协议的可靠性。
2. BGP 进行域间的路由选择，对协议的稳定性要求非常高。因此用 TCP 协议的高可靠性来保证 BGP 协议的稳定性。
3. BGP 的对等体之间必须逻辑上连通，并进行 TCP 连接。目的端口号为 179，本地端口号任意。
4. BGP 支持无类别域间路由 CIDR。
5. 路由更新时，BGP 只发送更新的路由，大大减少了 BGP 传播路由所占用的带宽，适用于在 Internet 上传播大量的路由信息。
6. BGP 是一种距离矢量路由协议，从设计上避免了环路的发生。
7. AS 之间：BGP 通过携带 AS 路径信息标记途经的 AS，带有本地 AS 号的路由将被丢弃，从而避免了域间产生环路。
8. AS 内部：BGP 在 AS 内学到的路由不会在 AS 中转发，避免了 AS 内产生环路。
9. BGP 提供了丰富的路由策略，能够对路由实现灵活的过滤和选择。
10. BGP 提供了防止路由振荡的机制，有效提高了 Internet 网络的稳定性。
11. BGP 易于扩展，能够适应网络新的发展。

### BGP 消息类型

1. Open 消息：Open 消息是 TCP 连接建立后发送的第一个消息，用于建立 BGP 对等体之间的连接关系。
2. Keepalive 消息：BGP 会周期性地向对等体发出 Keepalive 消息，用来保持连接的有效性。
3. Update 消息：Update 消息用于在对等体之间交换路由信息。它既可以发布可达路由信息，也可以撤销不可达路由信息。
4. Notification 消息：当 BGP 检测到错误状态时，就向对等体发出 Notification 消息，之后 BGP 连接会立即中断。

### BGP 邻居状态

1. 空闲（Idle）：为初始状态，当协议激活后开始初始化，复位计时器，并发起第一个 TCP 连接，并开始倾听远程对等体所发起的连接，同时转向 Connect 状态。
2. 连接（Connect）：开始 TCP 连接并等待 TCP 连接成功的消息。如果 TCP 连接成功，则进入 OpenSent 状态；如果 TCP 连接失败，进入 Active 状态。
3. 行动（Active）：BGP 总是试图建立 TCP 连接，若连接计时器超时，则退回到 Connect 状态，TCP 连接成功就转为 Open sent 状态。
4. OPEN 发送（Open sent）：TCP 连接已建立，自己已发送第一个 OPEN 报文，等待接收对方的 Open 报文，并对报文进行检查，若发现错误则发送 Notification 消息报文并退回到 Idle 状态。若检查无误则发送 Keepalive 消息报文，Keepalive 计时器开始计时，并转为 Open confirm 状态。
5. OPEN 证实（Open confirm）：BGP 等待 Keepalive 报文，同时复位保持计时器。如果收到了 Keepalive 报文，就转为 Established 状态，邻居关系协商完成。如果系统收到一条更新或 Keepalive 消息，它将重新启动保持计时器；如果收到 Notification 消息，BGP 就退回到空闲状态。
6. 已建立（Established）：即建立了邻居（对等体）关系，路由器将和邻居交换 Update 报文，同时复位保持计时器。

### BGP 路由通告原则

1. 多条路径时，BGP Speaker 只选最优的给自己使用（负载均衡和 FRR 除外）。
2. BGP Speaker 只把自己使用的路由（最优路由）通告给相邻体。
3. BGP Speaker 从 EBGP 获得的路由会向自己所有 BGP 相邻体通告（包括 EBGP 和 IBGP）。
4. BGP Speaker 从 IBGP 获得的路由不向自己的 IBGP 相邻体通告（反射器除外）。
5. BGP Speaker 从 IBGP 获得的路由是否通告给自己的 EBGP 相邻体要根据 IGP 和 BGP 同步的情况来决定。
6. 当收到对端的 refresh 报文并且本端邻居支持 refresh 能力，BGP Speaker 将把自己所 有 BGP 路由通告给对等体。
7. GR 过程中，主备倒换方在 GR 结束时 BGP Speaker 会把自己所有 BGP 路由通告给对等体。

### BGP 属性编辑

1. 公认必遵（Well-Known Mandatory）

- ORIGIN（起源）：这个属性说明了源路由是怎样放到 BGP 表中的。有三个可能的源 IGP,EGP,以及 INCOMPLETE.路由器在多个路由选择的处理中使用这个信息。路由器选择具有最低 ORIGIN 类型的路径。
- AS_PATH（AS 路径）：指出包含在 UPDATE 报文中的路由信息所经过的自治系统的序列。
- Next_HOP(下一跳)声明路由器所获得的 BGP 路由的下一跳，对 EBGP 会话来说，下一跳就是通告该路由的邻居路由器的源地址。

2. 公认自决（Well-Known Discretionary）

- LOCAL_PREF(本地优先级)：本地优先级属性是用于告诉自治系统内的路由器在有多条路径的时候，怎样离开自治系统。本地优先级越高，路由优先级越高。
- ATOMIC_AGGREGATE(原子聚合)：原子聚合属性指出已被丢失了的信息。

3. 可选过渡（Optional Transitive）

- AGGREGATOR(聚合者)：此属性标明了实施路由聚合的 BGP 路由器 ID 和聚合路由的路由器的 AS 号。
- COMMUNITY(团体)：此属性指共享一个公共属性的一组路由器。

4. 可选非过渡（Optional Nontransitive）

- MED(多出口区分)：该属性通知 AS 以外的路由器采用哪一条路径到达 AS,它也被认为是路由的外部度量，低 MED 值表示高的优先级。
- ORIGINATOR_ID(起源 ID)：路由反射器会附加到这个属性上，它携带本 AS 路由器的路由器 ID，用以防止环路。
- CLUSTER_LIST(簇列表)：此属性显示了采用的反射路径。
