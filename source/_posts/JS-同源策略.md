---
title: 同源策略
date: 2019-4-16
tags: JS
author: 映雪
---

世事短如春梦，人情薄似秋云。不须计较苦劳心，万事原来有命。

<!--more-->

### 同源策略

- 是浏览器最核心也最基本的安全功能，会约束浏览器的行为。
- 只允许本域内的脚本读写本域内的资源，不允许访问本域外的资源。

### 同源要素

1. 协议
2. 域名
3. 端口

### 限制范围

1. 无法共享 cookie, localStorage, indexDB
2. 无法操作彼此的 dom 元素
3. 无法发送 ajax 请求

### 域

- 域（Domain）是 Windows 网络中独立运行的单位；
- 域之间相互访问则需要建立信任关系，信任关系是连接在域与域之间的桥梁；
- 当一个域与其他域建立了信任关系后，2 个域之间不但可以按需要相互进行管理，还可以跨网分配文件和打印机等设备资源，使不同的域之间实现网络资源的共享与管理.

### 跨域

- 突破同源策略的限制，在两个不同的域之间（非同源页面）实现资源交互。

### 跨域分类

1. 使用 Ajax 引发的跨域问题

- 当调用 Ajax 时：调用 Ajax 发送请求的页面 所在的域，和被请求页面所在的域不一致

2. 类似页面嵌入 ifream 引起的跨域问题

- 当操作 ifream 内引入的元素时：ifream 所属页面的域，和 ifream 引入页面的域不一致

### 跨域实现方法

1. JSONP

```js
document.onclick = function () {
  var url = 'http://127.0.0.1/1/jsonp5.php';
  jsonp(
    url,
    function (res) {
      console.log(res);
    },
    {
      user: 'root',
      pass: 704206198,
    },
  );
};

function jsonp(url, callback, data) {
  data = data ? data : {};
  var str = '';
  for (var i in data) {
    str = str + i + '=' + data[i] + '&';
  }

  var d = new Date();
  url = url + '?' + str + '=' + d.getTime();
  console.log(url);
  var script = document.createElement('script');
  script.src = url;
  document.body.appendChild(script);

  window.fnHtml = function (res) {
    //局部作用域的全局函数-向上传
    callback(res);
  };
}
```

2. CORS
3. 降域
4. postMessage
