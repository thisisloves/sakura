---
title: JS存储对象
date: 2019-4-22
tags: JS
author: 映雪
---

这个世界就是这么不公平，有人活在天堂，有人活在地狱！这个世界又是公平的，有人活在天堂也不快乐，有人活在地狱却很快乐！

<!--more-->

### webstorage

- webstorage 是本地存储，存储在客户端，**不会发往服务器**，包括 localStorage 和 sessionStorage。


  |          方法           |                       描述                       |
  | :---------------------: | :----------------------------------------------: |
  |         key(n)          |          返回存储对象中第 n 个键的名称           |
  |    getItem(keyname)     |                  返回指定键的值                  |
  | setItem(keyname, value) | 添加键和值，如果对应的值存在，则更新该键对应的值 |
  |   removeItem(keyname)   |                      移除键                      |
  |         clear()         |              清除存储对象中所有的键              |

### localStorage

- localStorage 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。

- localStorage 存取

```js
window.localStorage.setItem('name', '小明');
console.log(localStorage.getItem('name'));
localStorage.clear();
localStorage.removeItem('name');
```

### sessionStorage

- sessionStorage 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

- sessionStorage 存取 

```js
window.sessionStorage.setItem('name', '小明');
console.log(sessionStorage.getItem('name'));
sessionStorage.clear();
sessionStorage.removeItem('name');
```

### 区别

![截屏2022-03-10 下午3.18.19.png](/images/2022/03/10/MCOW7IrSo5Rjx2t.png)