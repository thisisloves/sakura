---
title: AJAX
date: 2019-4-9
tags: JS
author: 映雪
---

若今昔一别 一别永年 苍山负雪 浮生尽歇。

<!--more-->

### AJAX

- 通过 XmlHttpRequest 对象向服务器发异步请求，从服务器获得数据，然后用 javascript 来操作 DOM 更新页面的技术。
- 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

### AJAX 原理

![截屏2022-03-25 下午5.40.04.png](/images/2022/03/25/EsHGQ2ytZN95dxe.png)

### XMLHttpRequest 对象

- XMLHttpRequest 是 AJAX 的基础。
- XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

|              方法              |                     描述                     |
| :----------------------------: | :------------------------------------------: |
|     open(method,url,async)     | 规定请求的类型、URL 以及是否异步处理请求。   |
|          send(string)          |             将请求发送到服务器。             |
| setRequestHeader(header,value) |             向请求添加 HTTP 头。             |

### AJAX 服务器响应

|     属性     |            描述            |
| :----------: | :------------------------: |
| responseText | 获得字符串形式的响应数据。 |
| responseXML  | 获得 XML 形式的响应数据。  |

### AJAX onreadystatechange 事件

- 当请求被发送到服务器时，我们需要执行一些基于响应的任务。
- 每当 readyState 改变时，就会触发 onreadystatechange 事件。


|     属性     |            描述            |
| :----------: | :------------------------: |
|onreadystatechange	|存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。|
|readyState	 |存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。|
| status	| 200: "OK"  404: 未找到页面 |

- readyState 状态 

- 0: 请求未初始化
- 1: 服务器连接已建立
- 2: 请求已接收
- 3: 请求处理中
- 4: 请求已完成，且响应已就绪


### AJAX 的使用

1. 创建请求对象

```js
var AjAX = new XMLHttpRequest();
```

2. 设置请求参数

```js
AJAX.open('get', 'http://localhost/1902/ajax/data/get.php', true);
```

3. 观察状态

```js
ajax.onreadystatechange = function () {
  if (ajax.readyState == 4 && ajax.status == 200) {
    func_succ(ajax.responseText);
  } else if (ajax.readyState == 4 && ajax.status != 200) {
    //alert("ajax faild readyState:"+ajax.readyState+" status:"+ajax.status);
  }
};
ajax.send(null);
```

- 清除缓存

```js
ajaxGet('data/test.txt?t=' + new Date().getTime(), fn);
```


### AJAX 优缺点

1. 不需要插件支持（一般浏览器且默认开启 JavaScript 即可）；
2. 用户体验极佳（不刷新页面即可获取可更新的数据）；
3. 提升 Web 程序的性能（在传递数据方面做到按需发送，不必整体提交）；
4. 减轻服务器和带宽的负担（将服务器的一些操作转移到客户端）；
5. 不同版本的浏览器对 XMLHttpRequest 对象支持度不足(比如 IE5 之前)；
6. 前进、后退的功能被破坏（因为 Ajax 永远在当前页，不会记录前后页面）；
7. 搜索引擎的支持度不够（因为搜索引擎爬虫还不能理解 JS 引起变化数据的内容）
