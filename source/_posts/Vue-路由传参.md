---
title: Vue路由传参
date: 2019-5-28
tags: Vue
author: 映雪
---

我与春风皆过客，你携秋水揽星河。

<!--more-->

### 路由跳转方式

1. 声明式跳转

```html
<router-link to="/about">跳转</router-link>
```

2. 编程式跳转

```js
this.$router.push({ path: '/about' });
```

### 路由传参

1. params

- 通过路由属性中的 name 来确定匹配的路由，通过 params 来传递参数。

```
// 声明式
<router-link :to='{name:"About",params:{a:1}}'>跳转</router-link>
// 编程式
this.$router.push({ name: 'About', params: { a: 1 } });
```

- 注意：**页面刷新参数会丢失。**

-  获取参数

```js
this.$route.params
```


2. query

- 使用 path 来匹配路由，然后通过 query 来传递参数,query 传递的参数会显示在 url 后面。
- 刷新页面不会丢失参数。

```
// 声明式
<router-link :to='{path:"/about",query:{a:1}}'>跳转</router-link>
// 编程式
this.$router.push({ path: '/about', query:{a:1}});

```

-  获取参数

```js
this.$route.query
```


3. 路由

- 需要路由支持

```
// 声明式
<router-link :to='{path:`/about/${id}`}'>跳转</router-link>
// 编程式
this.$router.push({path: `/about/${id}`})
```

- 路由配置

```js
  {
    path: '/about/:id',
    name: 'About',
    component: () => import('../views/About.vue')
  }
```


-  获取参数

```js
this.$route.params.id
```