---
title: CSS盒模型
date: 2018-2-21
tags: CSS
author: 映雪
---

长道相依，几恨别离，锦绣断相思意，待人相惜。

<!--more-->

### CSS盒模型

- 将HTML页面中的布局元素看作是一个矩形的盒子，也就是一个盛装内容的容器。CSS盒子模型本质上是一个盒子，封装周围的HTML元素，它包括：边框，外边距，内边距和实际内容。

![截屏2022-06-01 下午3.01.19.png](/images/2022/06/01/DdZKcJCBp5mLHtg.png)

### CSS盒模型组成

1. Margin(外边距) - 清除边框外的区域，外边距是透明的。
2. Border(边框) - 围绕在内边距和内容外的边框。
3. Padding(内边距) - 清除内容周围的区域，内边距是透明的。
4. Content(内容) - 盒子的内容，显示文本和图像。

### CSS盒模型类型

1. 标准盒模型

![截屏2022-06-01 下午4.03.02.png](/images/2022/06/01/23OuzHQJVcx6G1F.png)


2. IE盒模型

![截屏2022-06-01 下午4.02.52.png](/images/2022/06/01/dx57GX2sVWiQ4O6.png)


### 盒模型设置

```css
#wrap{
    /*IE模型*/
    box-sizing: border-box;
    /*标准模型*/
    box-sizing: content-box;
}
```

### 区别

- 两者的区别在于content的不同，IE盒模型的content包括border、padding，而标准盒模型不包含。
- 平时使用比较多的盒模型为IE盒模型。

