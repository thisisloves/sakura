---
title: Java重写和重载
date: 2021-10-20
tags: Java
author: 映雪
---

树红苞浑烂漫，风轻日暧竞芳华。

<!--more-->

### 重写(Override)

- 重写是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。**即外壳不变，核心重写！**
- 重写的好处在于子类可以根据需要，定义特定于自己的行为。 也就是说子类能够根据需要实现父类的方法。

### 重写规则

1. 参数列表与被重写方法的参数列表必须完全相同。
2. 返回类型与被重写方法的返回类型可以不相同，但是必须是父类返回值的派生类（java5 及更早版本返回类型要一样，java7 及更高版本可以不同）。
3. 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为 public，那么在子类中重写该方法就不能声明为 protected。
4. 父类的成员方法只能被它的子类重写。
5. 声明为 final 的方法不能被重写。
6. 声明为 static 的方法不能被重写，但是能够被再次声明。
7. 子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为 private 和 final 的方法。
8. 子类和父类不在同一个包中，那么子类只能够重写父类的声明为 public 和 protected 的非 final 方法。
9. 重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
10. 构造方法不能被重写。
11. 如果不能继承一个类，则不能重写该类的方法。


```java
/*
 * 重写:子类重写父类的方法同名
 * */

// 一个java类中只允许有一个public修饰的类。
public class Overriding_Test {
  public static void main(String args[]){
    Dog2 dog = new Dog2();
    Dog2.eat();
    dog.eat();

    Dog3 dog1 = new Dog3();
    dog1.eat("11");
    dog1.active();
  }
}
 class Dog2{
  public static void eat(){
    System.out.println("小狗吃东西");
  }
  protected void sun(){
    System.out.println("小狗跑步");
  }
  // 方法可继承，不可在q其他package 使用

  private void sleep(){
    System.out.println("小狗睡觉");
  }
  // 方法无法继承,只能在类中使用
}

class Dog3 extends Dog2{
  public void eat(String food){
    System.out.println("小狗吃东西,小狗吃的"+food);
  }
  public void active(){
    Dog2.eat();
    super.sun();
  }
  }

```

### 重载(Overload)

- 重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。
- 每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。
- 最常用的地方就是构造器的重载。


### 重载规则

1. 被重载的方法必须改变参数列表(参数个数或类型不一样)；
2. 被重载的方法可以改变返回类型；
3. 被重载的方法可以改变访问修饰符；
4. 被重载的方法可以声明新的或更广的检查异常；
5. 方法能够在同一个类中或者在一个子类中被重载。
6. 无法以返回值类型作为重载函数的区分标准。

```java
/*
 * 重载:类中有同名的方法但是收参数不一样
 */

class  Dog_1 {
  public void eat (){
    System.out.println("小狗狗吃东西");
  }
  public void eat(String food){
    System.out.println("小狗狗吃东西,吃的是"+food);
  }
}
public class OverLoading_Test {
  public static void main(String args[]){
    Dog_1 dog = new Dog_1();
    dog.eat("d");
    dog.eat();
  }
}

```


### 区别
1. 方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。
2. 方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。
3. 方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。

![截屏2021-12-17 下午7.16.09.png](/images/2021/12/17/puhveCb74VWaylS.png)
