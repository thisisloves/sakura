---
title: Action
date: 2019-6-15
tags: Redux
author: 映雪
---

我愿化身为青石桥，受五百年风吹，五百年日晒，五百年雨淋，只求此女子从桥上走过。

<!--more-->

### Action

- Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。

```js
const ADD_NUMBER = 'ADD_NUMBER'
{
  type: ADD_NUMBER,
  text: 'Build my first Redux app'
}

```

### 构建 Action

- Action 本质上是普通对象。约定，action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作。多数情况下，type 会被定义成字符串常量。当应用规模越来越大时，建议使用单独的模块或文件来存放 action。

- 单独的模块或文件来定义 action type 常量并不是必须的，甚至根本不需要定义。对于小应用来说，使用字符串做 action type 更方便些。不过，在大型应用中把它们显式地定义成常量还是利大于弊的。

- 尽量减少在 action 中传递的数据。

```js
{
  type: 'addCount',
  count: num
  }
```

### Action 创建函数

- 在 Redux 中的 action 创建函数只是简单的返回一个 action:

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text,
  };
}
```

#### Action 编写

```js
/*
 * action 类型
 */

export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';

/*
 * 其它的常量
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE',
};

/*
 * action 创建函数
 */

export function addTodo(text) {
  return { type: ADD_TODO, text };
}

export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index };
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter };
}
```
