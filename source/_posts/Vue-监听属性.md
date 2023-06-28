---
title: Vue监听属性
date: 2019-5-24
tags: Vue
author: 映雪
---

世不遇你，生无可喜。


<!--more-->

### Watch 监听

- 实际上是用来监听 vue 实例中的数据变化。

### 普通监听

- 普通监听是基本的数据类型：数字，布尔值，字符串。

```html
<script>
export default {
  name: 'Watch',
  data() {
    return {
      string: '1',
    };
  },
  methods: {
    changeString() {
      this.string = '2';
    }
  },
  watch: {
     string (newValue,oldValue){
       console.log("string"+newValue,oldValue)
     },
};
</script>
```

- 简单监听字符串的变化,但是第一次数据绑定改变并不会被监听。

### immediate

- 确认是否以当前的初始值执行 handler 的函数。

```html
<script>
export default {
  name: 'Watch',
  data() {
    return {
      string: '1',
    };
  },
  methods: {
    changeString() {
      this.string = '2';
    }
  },
  watch: {
    string: {
      handler(newValue, oldValue) {
        console.log('string' + newValue, oldValue);
      },
      immediate: true,
    },
};
</script>
```

- 默认 immediate 为 false 第一次数据绑定不监听。

### 深度监听

- 深度监听是引用数据类型：对象。

```html
<script>
export default {
  name: 'Watch',
  data() {
    return {
      obj: {
        name: 'Tony',
        age: 50,
        children: [
          {
            name: '小明',
            age: 12,
          },
          {
            name: '小花',
            age: 5,
          },
        ],
      },
      arr: [1, 2, 3],
      string: '1',
    };
  },
  methods: {
    changeString() {
      this.string = '2';
    },
    changeArr() {
      this.arr[1] = 3;
    },
    changeObj() {
      this.obj.children[0].name = '小华';
    },
  },
  watch: {
    arr: {
      handler(newValue, oldValue) {
        console.log('arr' + newValue, oldValue);
      },
      immediate: true,
    },
    obj: {
      handler(newValue, oldValue) {
        console.log('obj' + newValue, oldValue);
      },
      immediate: true,
      deep: true,
    },
  },
};
</script>
```

### deep

- 为了发现对象内部值的变化，可以在选项参数中指定 deep: true。监听数组的变更不需要这么做。


### 注意

- 不应该使用箭头函数来定义watcher函数。理由是箭头函数绑定了父级作用域的上下文，所以this 将不会按照期望指向Vue实例。(methods同理)
