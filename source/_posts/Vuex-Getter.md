---
title: Vuex-Getter
date: 2019-7-19
tags: Vuex
author: 映雪
---

骗尽多情是戏文，骗过天下是忠贞。

<!--more-->

### Getter

- store 的计算属性。
- 可以 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数。

### Getter 使用

```js
import { createStore } from 'vuex';

export default createStore({
  state: {
    name: '222',
  },
  mutations: {
    changeName(state, payload) {
      state.name = payload.name;
    },
  },
  getters: {
    getNameLength: (state) => {
      return state.name.length;
    },
  },
  actions: {},
  modules: {},
});
```

### 组件访问

- 可通过 this.$store.getters 访问

```html
<template>
  <a> 这是vuex的state内容：{{ name }}，长度为{{this.$store.getters.getNameLength}}></a>
  <a> 这是vuex的state内容：{{ name }}，长度为{{getNameLengthComputed}}></a>
</template>

<script>
  import { mapMutations, mapState } from 'vuex';
  export default {
    data() {
      return {};
    },
    computed: mapState({
      name: (state) => state.name,
    }),
    computed: {
      getNameLengthComputed() {
        return this.$store.getters.getNameLength;
      },
    },
  };
</script>
```

- Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  getNameLengthALL (state, getters) {
    return getters.getNameLength+1
  }
}
```

- 注意：getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。

### 通过方法访问

- getter 返回一个函数，来实现给 getter 传参。

```js
  getters:{
    getNameLength:(state)=>(value)=>{
      return state.name.length+value
    }
  },
```

- 组件使用

```html
<a> 这是vuex的state内容：{{ name }}，长度为{{this.$store.getters.getNameLength(2)}}></a>
```

- 注意：getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。

### mapGetters 辅助函数

- mapGetters 辅助函数将 store 中的 getter 映射到局部计算属性

```JS
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters({
      getNameLength:'getNameLength',
      // ...
    })
  }
}
```
