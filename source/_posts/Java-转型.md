---
title: Java转型
date: 2021-11-5
tags: Java
author: 映雪
---

花开两生面，人生佛魔间。

<!--more-->

### 转型

- 父子对象之间的转换分为了向上转型和向下转型

- 向上转型 : 通过子类对象(小范围)实例化父类对象(大范围),这种属于自动转换
- 向下转型 : 通过父类对象(大范围)实例化子类对象(小范围),这种属于强制转换

### 向上转型

- 上转型对象：子类创建对象 并将这个对象引用赋值给父类的对象。

```java
Father f=new Son();
```

- 注意事项：

1. 上转型对象是由子类创建的，但上转型对象会失去子类的一些属性和方法。
2. 上转型对象调用方法时，就是调用子类继承和重写过的方法。而不会是新增的方法，也不是父类原有的方法。
3. 上转型对象可以操纵父类原有的属性和功能，无论这些方法是否被重写。
4. 上转型对象可以再强制转换到一个子类对象，强制转换过的对象具有子类所有属性和功能。

```java
class A {
  public void print(){
    System.out.println("A:print");
  }
  public void print1(){
    System.out.println("A:print1");
  }
}
class B extends A {
  public void print(){
    System.out.println("B:print");
  }
  public void print2(){
    System.out.println("B:print2");
  }
}

public class Up_Test {
  public static void main (String args[]){
    B b = new B();
    A a = new B();  // 转型向上

    b.print();
    b.print2();
    a.print1();   // 调用A的print1
    a.print();   // 调用A的print，被子类重写过的
  }
}

```

### 向下转型

- 下转型对象：父类引用的对象转换为子类的类型（强制类型转换）。

```java
Father f = new Son();
Son s=(Son)f;
```

- 注意事项：

1. 向下转型必须先向上转型，否则会发生异常。
2. 下转型对象可以引用子类和父类的属性和方法。

```java

class A1 {
  public void print() {
    System.out.println("A:print");
  }
  protected void print1(){
    System.out.println("A:print1");
  }
}

class B1  extends A1 {
  public void print() {
    System.out.println("B:print");
  }
  protected void print2(){
    System.out.println("B:print");
  }
}
public class Down_Test {
  public static void main (String args[]){
    A1 a = new B1();
    B1 b = (B1)a;
    b.print1();   // A1的方法也可调用
    b.print2();
    b.print();
  }
}

```
