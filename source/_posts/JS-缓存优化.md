---
title: 缓存优化
date: 2021-10-15
tags: JS
author: 映雪
---

入我相思门，知我相思苦，长相思兮长相忆，短相思兮无穷极。

<!--more-->

![截屏2021-07-28 下午3.40.07.png](/images/2021/07/28/QN9VxWbeLm6yRUY.png)

### 浏览器缓存

- **浏览器通过HTTP请求网络资源后留在本地的一种行为，在页面上点击返回和前进的按钮就是利用浏览器的缓存。**

- 浏览器的缓存分为两种 强缓存和 协商缓存
- 浏览器缓存位置放在4个地方 Service Worker, Memory Cache, Disk Cache, Push Cache ,优先级由上往下。

### 浏览器请求资源全过程

- **先本地在服务器(先强缓存在协商缓存)**

- 浏览器请求资源时会先去判断本地缓存资源的header 是否有强缓存，如果有强缓存则直接请求本地资源，不向服务器发送请求。
- 如果没有强缓存或者强缓存失效后就会发送HTTP请求服务器，这个过程采用的就是协商缓存

- **简单来说浏览器的请求分为有无HTTP请求两种**

### 强缓存

- **所谓强缓存就是我们没有发送HT请求，而是直接从本地缓存中获取资源的一种行为，成功后返回状态码为200**

- 浏览器是根据响应头的headers字段判断Expires/http1.0,Cache-Control/http1.1,来执行强缓存的过程
- 没有或者失效的强缓存，浏览器会向服务器发送请求资源

#### Expires

- **http1.0 中一个页面的缓存字段，是一个格林时间。这个时间是浏览器强缓存资源失效的时间**

```
Expires: Wed, 22 Nov 2021 08:41:00 GMT
```
- 表示缓存的资源会在 2021年11月22号8点41分 过期。

* 缺点:浏览器是根据本地时间判断是否过期的，但是本地的时间可以被修改，所以在HTTP1.1时Expires被放弃了

#### Cache-Control

![截屏2021-07-28 下午4.44.37.png](/images/2021/07/28/BjXRPOlZCymeUf2.png)

- **HTTP1.1 中页面的缓存字段。 如果 Expires和Cache-Control 都存在，那么Cache-Control的优先级更高**

* Cache-Control 的属性值有很多，max-age表示一个相对缓存时间

```
Cache-Control:max-age = 3600

```
- 表示距离上次请求的一个小时内可以直接使用本地的缓存，不再请求

- 属性public表示可以被浏览器或者代理服务器缓存
- 属性private表示只能被浏览器缓存
- 属性no-cache需要发送请求到服务器确认是否被缓存，这次需要使用协商缓存
- 属性no-store表示禁用缓存，每一次都要请求缓存

### 协商缓存

- **所以协商缓存是指:浏览器携带缓存的标识tag请求服务器，服务器根据携带过来的标识判断是否使用缓存这个过程就是协商缓存**

- 浏览器请求返回结果有两种，一种304表示服务器资源还没有更新直接使用浏览器本地缓存即可。一种返回200，表示服务器资源更新且携带新的资源返回浏览器
- 缓存标识tag分为两种Last-Modified/If-Modified-Since 和Etag/If-None-Match, Etag/If-None-Match 的优先级要高于Last-Modified

![111.png](/images/2021/07/30/lhSgJUd5Lp3T9WR.png)

- Etag 时服务器响应请求的唯一标识，这个标识只能由服务器产生

```
etag: W/"5357d2b3f63545926812b95658505315"
```

- If-None-Match 是浏览器再次请求服务器时，会携带Etag标识值发给服务器，服务器就会将这个值和服务器中的Etag比较，两个值相等则返回304，如果不相等返回200
返回新的资源

- Last-Modified，指的是返回请求的资源文件最后在服务器被修改的时间。
- Last-Modified: Wed, 23 Nov 2021 08:41:00 GMT
- If-Modified-Since，是浏览器再次请求资源时，会携带上一次返回的 Last-Modified 的时间发送给服务器。服务器将上一次最后修改的时间 和现在的最后修改的时间做对比。如果大于If-Modified-Since 的值，服务器就会返回新的资源 200，否则返回 304

### 缓存位置

![11112222.png](/images/2021/07/30/wXRp4IsYaCJNWZE.png)

- **缓存的位置 Service Worker, Memory Cache, Disk Cache, Push Cache。Service Worker 优先级最高到 Push Cache** 

- Service Worker 运行在浏览器的独立线程可以实现浏览器的缓存功能，传输协议需要使用HTTPS。
- Memory Cache 是将资源缓存在内存中。
- Disk Cache 是将资源缓存在磁盘中
- Push Cache（推送缓存）是 HTTP/2 中，存活在会话session中，存活的时间很短。


### 本地存储

- **浏览器的本地缓存主要分为 5 种，localStorage, sessionStorage, cookie, WebSql, indexedDB。**

#### Cookie

- cookie 是服务器生成的，保存到浏览器的一个本地文件中。前端可以通过 Set-Cookie 设置 cookie，前端可以设置多个 Set-Cookie。
- cookie 可以设置过期的时间也可以不设置时间，浏览器关闭后就会失效。

```
Set-Cookie: BDSVRTM=7; path=/
Set-Cookie: H_PS_PSSID=34130_34099_33969_31253_33848_33607_26350; path=/; domain=.baidu.com
```

- cookie 产生的原因:是用来做状态存储的，因为http是无状态的，不能记录数据，cookie 可以记录数据的状态，比如id 用户名
- cookie 的优点: 1.记住数据的状态，密码等 2.弥补HTTP的无状态
- cookie 的缺点: 1.容量缺陷，只能储存4kb大小 2.安全问题，cookie是以文本的形式在浏览器和服务器之间传递信息，很有可能被修改 3.请求的cookie文件容易删除 4.性能消耗大，cookie是跟紧域名，域名下的任意地址修改都携带cookie到达服务器，造成性能浪费

#### Localstorage

- Localstorage 的存储方式和cookie类似，都会储存在同一个域名，localstorage可以长期储存没有时间期限。

- localstorage的优点 1.扩展了cookie的存储大小，可以存放5M大小左右，不同浏览器不同 2.只存储在浏览器不会和浏览器有通信解决了cookie性能问题和通信安全问题

- Localstorgae的缺点 1.需要手动删除保存的数据 2.只支持字符串类型数据，JSON类型需要通过JSON.strJSON.stringify()转化

- Localstorage使用场景:利用localstroge 可以存放一些稳定的资源base64 图片等

#### sessionStorage

- **sessionStorage和localstorage基本一致，唯一的区别就是sessionStorage是会话级别的存储** 

- 会话级别的存储sessionStorage，也就是在浏览器关闭后，这个存储就消失了
- sessionStorage 的场景:sessionStorage可以保存一些临时的数据


#### indexedDB

- **浏览器提供的非关系型数据库，indexedDB 提供大量的接口提供查询功能，还能建立查询。**

- 以键值对的形式存储值，包括 js 对象
- indexedDB 是异步的，存入数据不会导致页面卡顿。
- indexedDB 支持事务，事务是一系列操作过程中发生了错误，数据库会回退到操作事务之前的状态。
- 同源限制，不同源的数据库不能访问。
- 存储空间没有限制。


#### WebSql

- **已废弃，旨在通过js操控sql语句完成对数据的读写** 
