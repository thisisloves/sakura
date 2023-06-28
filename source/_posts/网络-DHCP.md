---
title: DHCP
date: 2019-4-6
tags: 网络
author: 映雪
---

古声澹无味，不称今人情。

<!--more-->

### DHCP (动态主机配置协议)

- DHCP（Dynamic Host Configuration Protoco）是一个局域网的网络协议。指的是由服务器控制一段 IP 地址范围，客户机登录服务器时就可以自动获得服务器分配的 IP 地址和子网掩码。默认情况下，DHCP 作为 Windows Server 的一个服务组件不会被系统自动安装，还需要管理员手动安装并进行必要的配置。

![截屏2022-06-20 下午5.39.28 1.png](/images/2022/06/20/MBjgYfAVTko6eHh.png)

- DHCP 服务对应的是 UDP 协议，使用 C/S 架构，客户机占用 UDP 67 号端口，DHCP 服务器占用 UDP 68 号端口。

### DHCP 功能

1. 保证任何 IP 地址在同一时刻只能由一台 DHCP 客户机所使用。
2. DHCP 应当可以给用户分配永久固定的 IP 地址。
3. DHCP 应当可以同用其他方法获得 IP 地址的主机共存（如手工配置 IP 地址的主机）。
4. DHCP 服务器应当向现有的 BOOTP 客户端提供服务。

### DHCP 分配方式

1. 自动分配方式（Automatic Allocation），DHCP 服务器为主机指定一个永久性的 IP 地址，一旦 DHCP 客户端第一次成功从 DHCP 服务器端租用到 IP 地址后，就可以永久性的使用该地址。
2. 动态分配方式（Dynamic Allocation），DHCP 服务器给主机指定一个具有时间限制的 IP 地址，时间到期或主机明确表示放弃该地址时，该地址可以被其他主机使用。
3. 手工分配方式（Manual Allocation），客户端的 IP 地址是由网络管理员指定的，DHCP 服务器只是将指定的 IP 地址告诉客户端主机。

### DHCP 原理

![截屏2022-06-20 下午5.47.47.png](/images/2022/06/20/4gKMxYUmzhXBE8C.png)

1. DHCP 客户机向局域网中所有 DHCP 服务器发送 DHCPdiscovery 请求。（DHCP 客户机向 DHCP 服务器发动 DHCP 请求，来索要 ip）
2. 局域网中所有的 DHCP 服务器都会回复 DHCPoffer，为客户机提供 IP 地址。
3. 客户机选择第一台 DHCP 服务器回复的 IP 地址，并且要发送 DHCPrequest 通告给局域网内所有的 DHCP 服务器，他选择了哪个 ip 和那个 DHCP 服务器。

**备注：客户机发送 DHCPrequest 通告的原因有两层。第一层是通告给所有 DHCP 服务器，让其他没有被选中的 DHCP 服务器把未使用的地址进行回收。第二层是通告被选中的 DHCP 服务器，确认此地址被 DHCP 客户机使用。**

4. 被选中的 DHCP 服务器收到 DHCPrequest 消息后，会给 DHCP 客户机回复一个 DHCPack，确认此 ip 可被客户机使用。客户机讲此 ip 与自身 MAC 绑定，以便下次继续使用，而其他 dhcp 服务器，将分配给该 DHCP 客户机的 ip 回收。

### DHCP 报文类型

|   报文类型    |                                含义                                |
| :-----------: | :----------------------------------------------------------------: |
| DHCP DISCOVER |                    客户端用来寻找 DHC 服务器。                     |
|  DHCP OFFER   | DHCP 服务器用来响应 DHCP DISCOVER 报文，此报文携带了各种配置信息。 |
| DHCP REQUEST  |                 客户端请求配置确认，或者续借租期。                 |
|   DHCP ACK    |                 服务器对 REQUEST 报文的确定响应。                  |
|   DHCP NAK    |                 服务器对 REQUEST 报文的拒绝响应。                  |
| DHCP RELEASE  |                 客户端要释放地址时用来通知服务器。                 |

### DHCP 租期

-  DHCP客户机使用IP地址是有限的。一般DHCP客户机使用DHCP分配得的IP地址到了租期的50%的时候，会主动的向DHCP服务器发出续约请求，DHCP服务器接受到DHCP客户机的续约请求后，DHCP服务器会检查此IP地址有没有别的DHCP客户机抢先占用，如果没有，则续约成功，如果此ip地址有被其他的DHCP客户机占用，那么续约就不成功。此时DHCP客户机将重新发起DHCPdiscovery请求来获取新的地址。

### DHCP 服务实现

1. DHCP服务器和DHCP客户机在同一网段

![截屏2022-06-21 上午10.05.43.png](/images/2022/06/21/u3dGR92rVIqNpyM.png)

```js
二层交换机1
[]vlan bat 10 20//创建vlan10 20
[]int e0/0/1//进入e0/0/1接口
[]port link-type access//设置access类型
[]port default vlan 10//接口划分vlan 10
[]int e0/0/2//进入e0/0/2接口
[]port link-type access//设置access类型
[]port default vlan 20//接口划分vlan 20
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
二层交换机2
[]vlan bat 10 20//创建vlan10 20
[]int e0/0/1//进入e0/0/1接口
[]port link-type access//设置access类型
[]port default vlan 10//接口划分vlan 10
[]int e0/0/2//进入e0/0/2接口
[]port link-type access//设置access类型
[]port default vlan 20//接口划分vlan 20
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
三层交换机
[]dhcp enable//开启DHCP服务
[]vlan bat 10 20//创建vlan 10 20
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all设置白名单
[]int g0/0/2//进入g0/0/2接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
 
方法一  接口下分发IP地址
[]int vlan 10   //进入vlanif10接口
[]ip add 192.168.10.1 255.255.255.0  //配置IP地址和子网掩码
[]dhep select interface //指定DHCP接口分发ip地址
[]dhcp server dns-list 4.4.4.4 8.8.8.8 //下发DNS服务器地址是4.4.4.4和8.8.8.8下发DNS服务器地址
方法二 建DHCP地址池
[]ip pool dhcp2 //新建一个DHCP地址池，名称为DHCP2
[]network 192. 168. 20. 0 mask 24 //指定DHCP2地址池的分发网段
[]gateway-list 192. 168. 20.1 //指定DHCP客户机获取到的网关地址
[]dns-list 2.2.2.2 40.40.40.40 //指定DHCP客户机获取的DNS服务器地址
[]lease day 9 //指定DHCP客户机可以使用的地址租期
[]int vlan 20 //进入vlanif20 接口
[]ip add 192. 168. 20. 1 255.255.255.0 //配置ip地址和子网掩码
[]dhep select global//接口下指定DHCP以地址池方式分配IP地址

```

2. DHCP服务器和DHCP客户机不在同一网段————DHCP中继，只能由路由器实现。

![截屏2022-06-21 上午10.06.12.png](/images/2022/06/21/9U4nHJOEx63wM8S.png)

```cs
各个设备间三条必打命令

<>sy//进入系统视图
[]sy 001//重命名
[]user-int co 0 
[]idle-timeout 0 0//永不超时
 
二层交换机1
[]vlan bat 10 20//创建vlan10 20
[]int e0/0/1//进入e0/0/1接口
[]port link-type access//设置access类型
[]port default vlan 10//接口划分vlan 10
[]int e0/0/2//进入e0/0/2接口
[]port link-type access//设置access类型
[]port default vlan 20//接口划分vlan 20
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
二层交换机2
[]vlan bat 10 20//创建vlan10 20
[]int e0/0/1//进入e0/0/1接口
[]port link-type access//设置access类型
[]port default vlan 10//接口划分vlan 10
[]int e0/0/2//进入e0/0/2接口
[]port link-type access//设置access类型
[]port default vlan 20//接口划分vlan 20
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
三层交换机
[]dhcp enable//开启DHCP服务
[]vlan bat 10 20 100//创建vlan 10 20 100
[]int g0/0/1//进入g0/0/1接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all设置白名单
[]int g0/0/2//进入g0/0/2接口
[]port link-type trunk//设置trunk类型
[]port trunk allow-pass vlan all//设置白名单
[]int g0/0/3//进入接口
[]port link-type access//设置access类型
[]port default vlan 100//把接口划分至vlan100
[]int vlan 10//进入vlanif10
[]ip add 192.168.10.1 24//配置IP地址和子网掩码长度
[]dhcp select relay //开启DHCP中继
[]dhcp relay server-ip 10.10.10.2 //设置DHCP服务器中继地址
[]int vlan 20//进入vlanif20
[]ip add 192.168.20.1 24//配置IP地址和子网掩码长度
[]dhcp select relay //开启DHCP中继
[]dhcp relay server-ip 10.10.10.2 //设置DHCP服务器中继地址
[]int vlan 100//进入接口
[]ip add 10.10.10.1 24//配置ip地址和子网掩码
DHCP服务器
[]dhcp enable//开启DHCP功能
[]int g0/0/0//进入接口g0/0/0
[]ip add 10.10.10.2 24//配置ip地址和子网掩码
[]undo shut//开启接口
[]dhcp select global//dhcp选择地址池方式分发ip
[]ip route-static 192.168.10.0 24 10.10.10.1//添加一条静态路由，目的网段是192.168.10.0，下一条10.10.10.1
[]ip route-static 192.168.20.0 24 10.10.10.1//添加一条静态路由，目的网段是192.168.20.0，下一条10.10.10.1
[]ip pool dhcp1//新建一个DHCP地址池，名称为DHCP1
[]network 192.168.10.0 mask 24 //指定DHCP2地址池的分发网段
[]gateway-list 192.168.10.1 //指定DHCP客户机获取到的网关地址
[]dns-list 20.20.20.20 9.9.9.9 //指定DHCP客户机获取的DNS服务器地址
[]lease day 9 //指定DHCP客户机可以使用的地址租期
[]ip pool dhcp2 //新建一个DHCP地址池，名称为DHCP2
[]network 192.168.20.0 mask 24 //指定DHCP2地址池的分发网段
[]gateway-list 192.168.20.1 //指定DHCP客户机获取到的网关地址
[]dns-list 2.2.2.2 40.40.40.40 //指定DHCP客户机获取的DNS服务器地址
[]lease day 9 //指定DHCP客户机可以使用的地址租期

```