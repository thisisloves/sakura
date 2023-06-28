---
title: Java多态
date: 2021-11-11
tags: Java
author: 映雪
---

归棹远，雨望雁，不许人间见白头。

<!--more-->

### Java 多态

![截屏2022-01-04 下午6.19.58.png](/images/2022/01/04/1kSMQFOGp7a5Rmu.png)

- 多态是同一个行为具有多个不同表现形式或形态的能力。
- 在同一个继承结构中使用统一的逻辑实现代码处理不同的对象，从而达到执行不同的行为
- 写一些只关注父类的代码, 就能够同时兼容各种子类的情况.

### 多态的优点

1. 消除类型之间的耦合关系
2. 可替换性
3. 可扩充性
4. 接口性
5. 灵活性
6. 简化性

### 多态存在的三个必要条件

1. 继承
2. 重写
3. 父类引用指向子类对象：Parent p = new Child();



### 代码实现

- 可以使程序有良好的扩展，并可以对所有类的对象进行通用处理。

```java
class Shape {
  public void draw (){
    System.out.println("啥也没有的父类");
  }
}
class Circle extends Shape{
  @Override
  public void draw() {
    System.out.println("圆");
  }
}
class Triangle extends Shape{
  @Override
  public void draw() {
    System.out.println("三角形");
  }
}
class Square extends Shape{
  @Override
  public void draw() {
    System.out.println("正方形");
  }
}

public class MoreStart_Test {
  public static void main (String args[]){
    Circle circle = new Circle();
    Triangle triangle = new Triangle();
    Square square  = new Square();

    circle.draw();
    triangle.draw();
    square.draw();

    // 向上转型
    Shape circle1 = new Circle();
    Shape triangle1 = new Triangle();
    Shape square1  = new Square();

    circle1.draw();
    triangle1.draw();
    square1.draw();

    Shape shape = new Shape();
    shape.draw();
  }
}
```

### 多态的实现方式

1. 方式一：重写：
2. 方式二：接口
3. 方式三：抽象类和抽象方法
