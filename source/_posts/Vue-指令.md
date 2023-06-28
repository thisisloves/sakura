---
title: Vue指令
date: 2019-5-21
tags: Vue
author: 映雪
---

往来皆过客，何曾有归人。

<!--more-->

### 指令

- 指令 (Directives) 是带有 v- 前缀的特殊特性。
- 指令特性的预期值是：单个 JavaScript 表达式。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。
- 例如 v-on，代表绑定事件。

### 插值表达式

- 是 vue 框架提供的一种在 html 模板中绑定数据的方式，使用{{变量名}}方式绑定 Vue 实例中 data 中的数据变量。会将绑定的数据实时的显示出来。

```html
<h3>{{ age>22 ? '成年':'未成年'}}</h3>
```

### 常用指令

1. v-html

- 将数据输出到元素内部，如果输出的数据有 HTML 代码，会被渲染

```html
<span v-html="active"></span>
<script>
  export default {
    data() {
      return { active: '<a>打游戏</a>' };
    },
  };
</script>
```

2. v-text

- 将数据输出到元素内部，会作为普通文本输出

```html
<span v-text="name"></span>
<!-- 和下面的一样 -->
<span>{{name}}</span>

<script>
  export default {
    data() {
      return { name: '小明' };
    },
  };
</script>
```

4. v-show

- 本质就是标签 display 设置为 none，控制隐藏

```html
<el-button @click="changeShow">v-show</el-button>
<div v-show="show">这是v-show</div>
<script>
  export default {
    data() {
      return { show: true };
    },
  };
</script>
```

5. v-if

- 本质是动态的向 DOM 树内添加或者删除 DOM 元素

```html
<el-button @click="changeIf">v-if</el-button>
<div v-if="iF">这是v-if</div>
<script>
  export default {
    data() {
      return { If: true };
    },
  };
</script>
```

6. v-for

- 根据一组数组的选项列表进行渲染

```html
<p v-for="(item,i) in arr" :key="i">数组索引值--{{ i }} -- 每一项--{{ item }}</p>
<script>
  export default {
    data() {
      return { arr: [1, 2, 3] };
    },
  };
</script>
```

7. v-on

- 绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略

```html
<button @click="fn()">click</button> <button v-on:click="fn()">click</button>
```

8. v-bind

- 动态地绑定一个或多个 attribute，或一个组件 prop 到表达式

```html
<img :src="img" />
<img v-bind:src="img" />
<script>
  export default {
    data() {
      return { img: 'https://cn.vuejs.org/images/logo.png' };
    },
  };
</script>
```

9. v-model

- vue 数据改变会修改视图改变，使用 v-model 就是在视图中修改数据，目前可使用 v-model 的元素，输入框，选择框，单选项，多选项，自定义组件。

```html
<input v-model="inputValue" />
<p>这是输入框内容:{{inputValue}}</p>
<script>
  export default {
    data() {
      return { inputValue: '' };
    },
  };
</script>
```
