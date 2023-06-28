---
title: BOM对象
date: 2019-3-28
tags: JS
author: 映雪
---

袭微雨，荡尽心中尘埃，缕清风，吹开陌上花红，如此，心在此岸已无岸，人在天涯已无涯。

<!--more-->

### BOM（Browser Object Model)

- 浏览器对象模型，浏览器对象模型提供了独立与内容的、可以与浏览器窗口进行互动的对象结构，BOM 由多个对象构成，其中代表浏览器窗口的 window 对象是 BOM 的顶层对象，其他对象都是该对象的子对象。

1. window 对象

- 是 JS 的最顶层对象，其他的 BOM 对象都是 window 对象的属性；

2. document 对象

- 文档对象

3. location 对象

- 浏览器当前 URL 信息

4. navigator 对象

- 浏览器本身信息；

5. screen 对象

- 客户端屏幕信息

6. history 对象

- 浏览器访问历史信息

### 原生对象和宿主对象

- 原生对象：Number、String、Boolean、Object、Array、Function、Date、RegExp、内置对象
- 宿主对象：window、BOM、DOM 等， web 的运行环境，即操作系统、浏览器,宿主提供的对象。

### window 对象

- BOM 的核心对象是 window，它表示浏览器的一个实例。
- 所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。
- 全局变量是 window 对象的属性。
- 全局函数是 window 对象的方法。

### window 对象方法

|                   方法                    |                         描述                          |
| :---------------------------------------: | :---------------------------------------------------: |
|        window.open(url,name,specs)        |                    返回一个新窗口                     |
|              window.close()               |                    关闭浏览器窗口                     |
|         window.prompt(对话框消息)         | 弹出输入框，点击确认，返回字符串，点击取消，返回 null |
|         window.confirm(提示信息)          |      弹出提示框，点击确定返回 true，点击取消返回      |
|           window.scrollTo(x,y)            |              把页面内容滚动到指定的坐标               |
|              window.scrollX               |               页面水平方向滚动的像素值                |
|              window.scrollY               |              页面垂直方向滚动的像素值　               |
| window.setInterval ( 函数/名称 , 毫秒数 ) |   表示每经过一定的毫秒后，执行一次相应的函数(重复)    |
| window.setTimeout ( 函数/名称 , 毫秒数 )  |  表示经过一定的毫秒后，只执行一次相应的函数(不重复)   |

### window 对象下的内置子对象

1. history 对象

- history 对象包含有关用户的访问历史记录

|    属性或方法     |                         描述                         |
| :---------------: | :--------------------------------------------------: |
|      length       |           返回浏览器历史列表中的 URL 数量            |
|  history.back()   |                    后退加一个 url                    |
| histroy.forward() |       前进，需要后退一下之后，才会有前进的方向       |
|  histroy.go(num)  | 参数为正，前进相应的数目，为负，后退相应的数目，为 0 |

2. location 对象

- location 对象包含有关当前页面的 URL 信息

|    属性或方法     |                            描述                            |
| :---------------: | :--------------------------------------------------------: |
|   location.href   |                   设置或者返回完整的 url                   |
|  location.search  |                  返回 url?后面的查询部分                   |
|   location.hash   |   是一个可读的字符串，是 url 的锚部分(从#开始的部分)哈希   |
| location.reload() | 参刷新页面的方法，一般情况下，传递一个 true,不使用缓存刷新 |

3. navigator 对象

- navigator 对象用于提供与用户浏览器相关的信息

|       属性或方法        |                      描述                      |
| :---------------------: | :--------------------------------------------: |
|  navigator.appCodeName  |               返回浏览器的代码名               |
|    navigator.appName    |                返回浏览器的名称                |
| navigator.cookieEnabled |    返回指明浏览器中是否启用 cookie 的布尔值    |
|   navigator.platform    |          返回运行浏览器的操作系统平台          |
|  navigator.appVersion   |           返回浏览器的平台和版本信息           |
|  navigator.appVersion   | 返回用户浏览器发送服务器的 user-agent 头部的值 |

4. screen 对象

- screen 对象包含有关客户端显示屏幕的信息

| 属性或方法  |                      描述                      |
| :---------: | :--------------------------------------------: |
|    width    |            属性返回显示器屏幕的宽度            |
|   height    |            属性返回显示器屏幕的高度            |
| availHeight | 属性返回显示屏幕的高度 (除 Windows 任务栏之外) |
| availWidth  | 属性返回显示屏幕的宽度 (除 Windows 任务栏之外) |

5. document 对象

- Document 对象是 Window 对象的一部分，可通过 window.document 属性对其进行访问
- 每个载入浏览器的 HTML 文档都会成为 Document 对象
- document 对象与它所包含的各种节点构成了早期的文档对象模型（DOM 0 级）
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素

### window 事件

1. window.onload

- 当文档加载完成后执行一些操作

```js
window.onload = function () {
  console.log('页面加载完成');
};
```

2. window.onscroll

- 当页面发生滚动时执行一些操作

```js
window.onscroll=function(){ console.log(1) //当页面发生滚动时，打印 1}
```

3. window.onresize

- 当窗口发生大小改变的时候执行一些操作

```js
window.onresize = function（）{console.log（"1")}
```
