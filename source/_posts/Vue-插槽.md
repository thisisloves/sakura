---
title: Vue插槽
date: 2019-6-3
tags: Vue
author: 映雪
---

雁字回时，月满西楼。花自飘零水自流。

<!--more-->

### 插槽

- 是组件内部留一个或多个的插槽位置，可供组件传对应的模板代码进去。

### 匿名插槽

- 父组件

```vue
<template>
  <div class="father">
    <BoxItem title="盒子1">
      <div>这是匿名插槽</div>
    </BoxItem>
  </div>
</template>
<script>
import BoxItem from '@/components/BoxItem.vue';

export default {
  name: 'Fatcher',
  components: {
    BoxItem,
  },
};
</script>
```

- 子组件

```vue
<template>
  <div class="box-item">
    <div class="box-header">{{title}}</div>
    <div class="box-content">
     <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BoxItem',
  props: {
    title: String,
  }
}
</script>
```

### 具名插槽

- 父组件中放入带有 slot 属性的内容，然后这些内容就会被分发到子组件中特殊的 slot 元素，根据 name 属性在子组件中重新组合，替换。

```vue
<template>
  <div class="father">
    <BoxItem title="盒子1">
      <template v-slot:con> 这是具名插槽 </template>
    </BoxItem>
  </div>
</template>

<script>
import BoxItem from '@/components/BoxItem.vue';

export default {
  name: 'Fatcher',
  components: {
    BoxItem,
  },
};
</script>
```


- 子组件

```vue
<template>
  <div class="box-item">
    <div class="box-header">{{title}}</div>
    <div class="box-content">
     <slot name="con"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BoxItem',
  props: {
    title: String,
  }
}
</script>
```

### 作用域插槽

- 可以理解为子组件可以将自己的数据发给父组件来处理。

- 子组件传值

```vue
<template>
  <div class="box-item">
    <div class="box-header">{{title}}</div>
    <div class="box-content">
     <slot name="con" :array=array></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BoxItem',
  data(){
    return {
      array:[1,2,3]
    }
  },
  props: {
    title: String,
  }
}
</script>
```

- 父组件操作数据

```vue
<template>
  <div class="father">
    <BoxItem title="盒子1">
      <template v-slot:con="props">
         传递的参数:{{props}} 
        <p v-for="(item ,index) in props.array" :key="index">{{item}}</p>
        </template>
    </BoxItem>
  </div>
</template>

```


- 首先在子组件的slot上动态绑定一个值(:key='value')
- 然后在父组件通过v-slot : name = 'values'的方式将这个值赋值给 values
- 最后通过{{ values.key }}的方式获取数据