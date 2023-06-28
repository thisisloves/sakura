---
title: Vue计算属性
date: 2019-7-9
tags: Vue
author: 映雪
---

看尽人间繁华，三千浮生若水。

<!--more-->

### 计算属性

- 通过计算得出的属性就是计算属性，计算属性可以是一个函数或者是一个getter和setter组成的对象。
- 模版内的表达式设计为了简单运算，对于任何复杂逻辑，都应当使用计算属性。

1. 使用methods

```vue
<template>
  <div class="computed">
    <table border="1">
      <thead>
        <th>学科</th>
        <th>分数</th>
      </thead>
      <tbody>
        <tr>
          <td>数学</td>
          <td><input type="number" v-model="math" /></td>
        </tr>
        <tr>
          <td>英语</td>
          <td><input type="number" v-model="english" /></td>
        </tr>
        <tr>
          <td>化学</td>
          <td><input type="number" v-model="chemistry" /></td>
        </tr>
        <tr>
          <td>总分</td>
          <td>{{ sum() }}</td>
        </tr>
        <tr>
          <td>平均分</td>
          <td>{{ avg() }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Computed',
  data() {
    return {
      math: 88,
      english: 77,
      chemistry: 99,
    };
  },
 methods:{
    sum() {
      return this.math + this.english + this.chemistry;
    },
    avg() {
      return this.sum()/ 3;
    }
 },
};
</script>
```


2. 使用computed

```vue
<template>
  <div class="computed">
    <table border="1">
      <thead>
        <th>学科</th>
        <th>分数</th>
      </thead>
      <tbody>
        <tr>
          <td>数学</td>
          <td><input type="number" v-model="math" /></td>
        </tr>
        <tr>
          <td>英语</td>
          <td><input type="number" v-model="english" /></td>
        </tr>
        <tr>
          <td>化学</td>
          <td><input type="number" v-model="chemistry" /></td>
        </tr>
        <tr>
          <td>总分</td>
          <td>{{ sum }}</td>
        </tr>
        <tr>
          <td>平均分</td>
          <td>{{ avg}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Computed',
  data() {
    return {
      math: 88,
      english: 77,
      chemistry: 99,
    };
  },
  computed: {
    sum() {
      return this.math + this.english + this.chemistry;
    },
    avg() {
      return this.sum / 3;
    },
  },
};
</script>
```

### 计算属性优点

- 计算结果发生变化，重新运行计算，进行页面更新。计算属性的结果会被缓存，计算属性只有在它的相关依赖发生改变时才会重新计算。提高了程序的性能。
- 计算依赖数据，当data中的数据改变时才会重新计算。

