---
title: OSI七层模型
date: 2016-1-2
tags: 网络
author: 映雪
---

夜阑风静欲归时，惟有一江明月碧琉璃。

<!--more-->

### OSI (开放式系统互联)

- Open System Interconnect 开放式系统互联，ISO（国际标准化组织）组织研究的网络互连模型。此构造标准定义了网络通信互联的七层框架（物理层、数据链路层、网络层、传输层、会话层、表示层和应用层）。

![截屏2022-06-15 下午2.35.42.png](/images/2022/06/15/ERNdWF8jcMibHS1.png)

### OSI 分层

1. 物理层

- 主要定义物理设备标准，如网线的接口类型、光纤的接口类型、各种传输介质的传输速率等。它的主要作用是传输比特流（就是由 1、0 转化为电流强弱来进行传输,到达目的地后在转化为 1、0，也就是我们常说的数模转换与模数转换）。这一层的数据叫做比特。

2. 数据链路层

- 定义了如何让格式化数据以进行传输，以及如何让控制对物理介质的访问。这一层通常还提供错误检测和纠正，以确保数据的可靠传输。这一层数据叫做帧。

3. 网络层

- 在位于不同地理位置的网络中的两个主机系统之间提供连接和路径选择。Internet 的发展使得从世界各站点访问信息的用户数大大增加，而网络层正是管理这种连接的层。这一层数据叫做报文。

4. 传输层

- 定义了一些传输数据的协议和端口号，如 TCP 和 UDP。 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组。这一层数据叫做段。

5. 会话层

- 通过传输层（端口号：传输端口与接收端口）建立数据传输的通路。主要在你的系统之间发起会话或者接受会话请求（设备之间需要互相认识可以是 IP 也可以是 MAC 或者是主机名）。

6. 表示层

- 可确保一个系统的应用层所发送的信息可以被另一个系统的应用层读取。例如，PC 程序与另一台计算机进行通信，其中一台计算机使用扩展二一十进制交换码（EBCDIC），而另一台则使用美国信息交换标准码（ASCII）来表示相同的字符。如有必要，表示层会通过使用一种通格式来实现多种数据格式之间的转换。


### OSI 分层对应协议

![截屏2022-06-15 下午2.56.26.png](/images/2022/06/15/d34uQzmKFYWOPj7.png)

### OSI 数据封装

![截屏2022-06-15 下午2.58.51.png](/images/2022/06/15/NVaUwKfDxpY86b2.png)

- 应用层：原始数据被转换成二进制数据
- 传输层：二进制数据被分割成小的数据段，并封装TCP头部 （数据段）（TCP头部的关键信息–端口号）
- 网络层：传输层传来的数据被封装上IP头部 （数据包）（IP头部的关键信息–IP地址）
- 数据链路层：网络层传来的数据被封装上MAC头部 （数据帧）（MAC头部的关键信息–MAC地址）
- 物理层：二进制数据组成的比特流转化为电信号在网络中传输 （比特流）


### OSI 数据解封装

![截屏2022-06-15 下午2.59.00.png](/images/2022/06/15/IqydmU38V4wpWgC.png)

- 物理层：将电信号转化为二进制数据，并将其送至数据链路层
- 数据链路层：查看MAC地址，地址是自己，就拆掉MAC头部，继续传输，否则就丢弃数据
- 网络层：查看IP地址，地址是自己，就拆掉IP头部，继续传输，否则就丢弃数据
- 传输层：查看TCP头部，判断应该传到哪里，然后重组数据，传输到应用层
- 应用层：二进制转化为原始数据
