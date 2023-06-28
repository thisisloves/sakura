---
title: Vue导航守卫
date: 2019-8-25
tags: Vue
author: 映雪
---

红藕香残玉簟秋，轻解罗裳，独上兰舟。云中谁寄锦书来？

<!--more-->

### 导航守卫

- vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。

### 全局前置守卫（router.beforeEach）

```js
const router = createRouter({ ... })

router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  return false
})

```

- 当一个导航触发的时候，全局前置导航守卫按照顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于等待中。

- 全局前置守卫参数

|    参数     |                             描述                              |
| :---------: | :-----------------------------------------------------------: |
|     to      |              即将要进入的目标的路由对象                 |
|    from     |               当前导航正要离开的路由                  |
| next (可选) | 用该方法来 resolve 这个钩子，执行效果依赖 next 方法的调用参数 |

1. to:Route 即将要进入的目标的路由对象
2. from：Route 当前导航正要离开的路由
3. next:Function 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数

- next不同参数执行效果

1. next() 进行管道的下一个钩子函数，如果所有钩子执行完，则导航状态就是 confirmed(确认的)。
2. next(false) 中断当前的导航。如果浏览器的 URL 改变了(可能是用户手动或者浏览器后退按钮)，那么 URL 会重置到 from 路由对应的地址
3. next('/')或者 next({path:'/'}) 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。
4. next(error) 如果传入 next 的参数是一个 Error 实例，则导航会被终止，且该错误会传递给 router.onError()注册过的回调。

- 确保要调用 next 方法，否则钩子就不会被 resolved

```js
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

### 全局解析守卫 (router.beforeResolve )

- 每次导航时都会触发，但是确保在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被正确调用。

```js
router.beforeResolve(async to => {
  if (to.meta.requiresCamera) {
    try {
      await askForCameraPermission()
    } catch (error) {
      if (error instanceof NotAllowedError) {
        // ... 处理错误，然后取消导航
        return false
      } else {
        // 意料之外的错误，取消导航并把错误传给全局处理器
        throw error
      }
    }
  }
})
```


### 路由导航守卫（beforeEnter）

-  直接在路由配置上定义 beforeEnter 守卫，只在进入路由时触发，不会在 params、query 或 hash 改变时触发。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        if (to.path !== '/user/login' && to.path !== '/user/nologin') {
          if (localStorage.getItem('isLogin') === 'ok') {
            next('/user/login');
          } else {
            next('/user/nologin');
          }
        } else {
          next();
        }
      },
    },
  ],
});
```

### 组件内的导航守卫（beforeRouteEnter,beforeRouteUpdate,beforeRouteLeave）

- 路由组件内直接定义路由导航守卫(传递给路由配置的)

```js
const Foo = {
  tempalte: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件对应路由被confirm 前调用
    // 不能获取组件实例 this
    // 因为当前守卫执行前，组件实例还没有被创建
  },
  beforeRouterUpdata(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id 在 /foo/1和 /foo/2 之间跳转的时候
    // 由于会渲染同样的 Foo 组件,因此组件的实例会被复用。而这个钩子就会在这个情况下被调用
    // 可以访问组件的实例 this
  },
  beforeROuteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 this
  },
};
```

- beforeRouteEnter 守卫 不能 访问 this，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。
- 不过，你可以通过传一个回调给 next 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

- 注意 beforeRouteEnter 是支持给 next 传递回调的唯一守卫。对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

- 这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 next(false) 来取消。

```js
beforeRouteLeave (to, from , next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```


### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。
