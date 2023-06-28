---
title: Vue路由
date: 2019-5-22
tags: Vue
author: 映雪
---

半帘诗画醉烟柳，国色倾城花满楼。

<!--more-->

### 路由核心

- 改变视图的同时不会向后端发出请求。


### hash 路由模式

- hash特点

1. hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
2. hash 值变化不会导致浏览器向服务器发出请求，而且 hash 改变会触发 hashchange 事件，浏览器的进后退也能对其进行控制。


- vue的hash模式

```js
import { createRouter, createWebHashHistory } from 'vue-router'

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
})
```

- hash 例子

1. 网易云音乐:http://music.163.com/#/friendhttps://pan.baidu.com/disk/home#list/vmode=list


### history 路由模式

- history 特点

1. 去除了#符号
3. 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持)
2. hash 的传参是基于 url 的，如果要传递复杂的数据，会有体积的限制，而 history 模式不仅可以在url里放参数，还可以将数据存放在一个特定的对象中。
4. 前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.abc.com/book/id。如果后端服务器缺少对 /book/id 的路由处理，将返回 404 错误。

- history 优势

1. pushState() 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 hash 只可修改 # 后面的部分，因此只能设置与当前 URL 同文档的 URL。
2. pushState() 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 hash 设置的新值必须与原来不一样才会触发动作将记录添加到栈中。
3. pushState() 通过 stateObject 参数可以添加任意类型的数据到记录中；而 hash 只可添加短字符串。
4. pushState() 可额外设置 title 属性供后续使用。


- vue的history模式

```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    //...
  ],
})
```

