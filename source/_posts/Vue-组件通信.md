---
title: Vue组件通信
date: 2019-5-27
tags: Vue
author: 映雪
---

时光清浅处，一步一安然。

<!--more-->

### 组件通信

- 组件实例的作用域是相互独立的，这就意味着不同组件之间的数据无法相互引用。因此组件与组件之间需要有通信。
- 子组件的关系可以总结为 props 向下传递，事件向上传递。父组件通过 props 给子组件下发数据，子组件通过事件给父组件发送信息。

### props 通信

- 将子组件所需的参数由父组件传输至子组件。

```html
<template>
  <div class="fatcher">
    <Child :string="string" :obj="obj" :arr="arr" :changeArr="changeArr" />
    <el-button @click="changeString">点击修改字符串</el-button>
    <el-button @click="changeObj">点击修改对象</el-button>
    <el-button @click="changeArr">点击修改数组</el-button>
    <div>string值为:{{ string }}</div>
    <p v-for="(item, i) in arr" :key="i">数组索引值{{ i }} ， 每一项{{ item }}</p>
    <p v-for="(val, key) in obj" :key="key">对象键是{{ key }} ，值是{{ val }}</p>
  </div>
</template>

<script>
import Child from '@/components/Child.vue';

export default {
  name: 'Fatcher',
  data() {
    return { string: '', obj: {}, arr: [] };
  },

  methods: {
    changeString() {
      this.string = 'string';
    },
    changeObj() {
      this.obj = { a: '1', b: '2' };
    },
    changeArr() {
      this.arr[0] = '1';
      this.arr[1] = '2';
    },
  },
  components: {
    Child,
  },
};
</script>
```

- 在子组件接收，并通过事件给父组件传输信息。

```html
<template>
  <el-button @click="changeArr">修改数组</el-button>
  <div>string值为:{{ string }}</div>
  <p v-for="(item, i) in arr" :key="i">数组索引值{{ i }} ， 每一项{{ item }}</p>
  <p v-for="(val, key) in obj" :key="key">对象键是{{ key }} ，值是{{ val }}</p>
</template>

<script>
export default {
  name: 'Child',
  props: {
    string: String,
    changeArr: Function,
    obj: Object,
    arr: Array,
  },
};
</script>
```

### $ref 实现通信

- ref 是被用来给元素或子组件注册引用信息的。引用信息将会注册在父组件的 $refs 对象上。

- 给子组件注册引用信息

```html
<template>
  <div class="fatcher">
    <Child :changeArr="changeArr" :changeObj="changeObj" :changeString="changeString" ref="child" />
  </div>
</template>

<script>
import Child from '@/components/Child.vue';

export default {
  name: 'Fatcher',
  data() {
    return {};
  },
  methods: {
    changeString() {
      this.$refs.child?.changeString();
    },
    changeObj() {
      this.$refs.child?.changeObj();
    },
    changeArr() {
      this.$refs.child?.changeArr();
    },
  },
  components: {
    Child,
  },
};
</script>
```

- 子组件

```html
<template>
  <el-button @click="changeString">点击修改字符串</el-button>
  <el-button @click="changeObj">点击修改对象</el-button>
  <el-button @click="changeArr">点击修改数组</el-button>
  <div>string值为:{{ string }}</div>
  <p v-for="(item, i) in arr" :key="i">数组索引值--{{ i }} --每一项--{{ item }}</p>
  <p v-for="(val, key) in obj" :key="key">对象键是--{{ key }}--值是--{{ val }}</p>
</template>

<script>
export default {
  name: 'Child',
  data() {
    return { string: '', obj: {}, arr: [] };
  },
  methods: {
    changeString() {
      this.string = 'string';
    },
    changeObj() {
      this.obj = { a: '1', b: '2' };
    },
    changeArr() {
      this.arr[0] = '1';
      this.arr[1] = '2';
    },
  },
};
</script>
```

#### props 和$ref 之间的区别

- props 着重于数据的传递，它并不能调用子组件里的属性和方法。像创建文章组件时，自定义标题和内容这样的使用场景，最适合使用 props。
- $ref 着重于索引，主要用来调用子组件里的属性和方法，其实并不擅长数据传递。而且 ref 用在 dom 元素的时候，能使到选择器的作用，这个功能比作为索引更常有用到。
- **$ref 无法反向调用父组件的方法，实际相当于父组件有了子组件的作用域。**

### $emit 通信

- $emit 绑定一个自定义事件 event，当这个这个语句被执行到的时候，就会将参数 arg 传递给父组件，父组件通过@event 监听并接收参数。

```html
<template>
  <div class="fatcher">
    <Child @changeString="changeString" />
    <div>string值为:{{ string }}</div>
  </div>
</template>

<script>
import Child from '@/components/Child.vue';

export default {
  name: 'Fatcher',
  data() {
    return { string: '', obj: {}, arr: [] };
  },
  methods: {
    changeString(value) {
      this.string = value;
    },
  },
  components: {
    Child,
  },
};
</script>
```

```html
<template>
  <el-button @click="changeString">点击修改字符串</el-button>
</template>

<script>
export default {
  name: 'Child',
  methods: {
    changeString() {
      this.$emit('changeString', '修改的string');
    },
  },
};
</script>
```
