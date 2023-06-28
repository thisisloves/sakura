---
title: mock
date: 2019-6-12
tags: JS
author: 映雪
---

彼春花香，此下尤伤，今秋已凉，来冬甚慌，这时光，恰如我思念你的模样。

<!--more-->

### Mock

```
cnpm i mockjs -D
```

- 在 src 目录下创建一个文件夹 mock/index.js

```js
const Mock =require('mockjs');

function getBannerFn() {
  let arr =[];
  for(var i=0;i<10;i++){
    arr.push({
      id:'banner'+i,
      imgSrc:Mock.Random.image('100x100',Mock.mock('@color()')),
      alt:'',
      href:''
    })
  }
  return {
    code:200,
    message:'ok',
    data:arr
  }
}

function getProlist() {
let arr=[]

 for (var i=0;i<50;i++){
  arr.push({
    id:'pro'+i,
    name:Mock.mock('@ctitle()'),
    imgSrc:Mock.Random.image('100x100',Mock.mock('@color()')),
    price:Math.random()*450+50,
    color:Mock.mock('@color()')
    address:Mock.mock('@county(true)')
  })
}
return arr
}
Mock.mock('http://47.103.82.2:3000/getBanner','get',getBannerFn);
Mock.mock('http://47.103.82.2:3000/getList','get',getProlist)
```

- 如果是 vue(或者 React)项目，那么就在入口文件处引入如下的语句。

```js
import './mock/index'
```

- 在需要的组件中，请求数据即可。

```js
axios.get('http://47.103.82.2:3000/getBanner').then(res=>{
  console.log(res.data)
  //后续的业务逻辑
})

```

- 如果后端接口全部准备就绪，将将 mock 的数据替换为真实的数据，只需要把入口页面处引入的 mockjs 文件删掉即可（前提条件 mock 的接口和数据的定义和真实的接口是一样的。
