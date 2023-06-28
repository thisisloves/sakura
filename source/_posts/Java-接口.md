---
title: Java接口
date: 2021-11-27
tags: Java
author: 映雪
---

愿作深山木，枝枝连理生。

<!--more-->

### 接口

- 一个抽象类型，是抽象方法的集合，接口通常以 interface 来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
- 接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念。类描述对象的属性和方法。接口则包含类要实现的方法。
- 除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。
- 接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。

```java
interface  Interface_Test {
  public  String name = "cxm";
  final String address ="china";
  static int age = 18;
  public void eat();
  public void travel();
}
```

### 接口的实现

- 一个类可以同时实现多个接口。
- 一个类只能继承一个类，但是能实现多个接口。
- 一个接口能继承另一个接口，这和类之间的继承比较相似。

```java
public class Interface_Example  implements Interface_Test,Interface_Test2 {
  public static void main(String [] args){
   Interface_Example i = new Interface_Example();
   i.eat();
   i.eat("4");
   i.travel();
   i.speak();
  }
  @Override
  public void eat(){
    System.out.println("一生何求-1");
    System.out.println(Interface_Test.age);
    System.out.println(Interface_Test.name);
    System.out.println(Interface_Test.address);
  }
  @Override
  public void travel() {
    System.out.println("一生何求-2");
  }
  public void eat(String food){
    System.out.println("一生何求-"+food);
  }

  @Override
  public void speak() {
    System.out.println("一生何求-3");
  }
}
```

### 接口的继承

```java
// 文件名: Sports.java
public interface Sports
{
   public void setHomeTeam(String name);
   public void setVisitingTeam(String name);
}

// 文件名: Football.java
public interface Football extends Sports
{
   public void homeTeamScored(int points);
   public void visitingTeamScored(int points);
   public void endOfQuarter(int quarter);
}

// 文件名: Hockey.java
public interface Hockey extends Sports
{
   public void homeGoalScored();
   public void visitingGoalScored();
   public void endOfPeriod(int period);
   public void overtimePeriod(int ot);
}
```

### 接口和类相同点

1. 一个接口可以有多个方法。
2. 接口文件保存在 .java 结尾的文件中，文件名使用接口名。
3. 接口的字节码文件保存在 .class 结尾的文件中。
4. 接口相应的字节码文件必须在与包名称相匹配的目录结构中。

### 接口与类的区别

1. 接口不能用于实例化对象。
2. 接口没有构造方法。
3. 接口中所有的方法必须是抽象方法，Java 8 之后 接口中可以使用 default 关键字修饰的非抽象方法。
4. 接口不能包含成员变量，除了 static 和 final 变量。
5. 接口不是被类继承了，而是要被类实现。
6. 接口支持多继承

### 抽象类和接口的区别

1. 抽象类中的方法可以有方法体，就是能实现方法的具体功能，但是接口中的方法不行。
2. 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 public static final 类型的。
3. 接口中不能含有静态代码块以及静态方法(用 static 修饰的方法)，而抽象类是可以有静态代码块和静态方法。
4. 一个类只能继承一个抽象类，而一个类却可以实现多个接口

### 接口特性

1. 接口中每一个方法也是隐式抽象的,接口中的方法会被隐式的指定为 public abstract（只能是 public abstract，其他修饰符都会报错）。
2. 接口中可以含有变量，但是接口中的变量会被隐式的指定为 public static final 变量（并且只能是 public，用 private 修饰会报编译错误）。
3. 接口中的方法是不能在接口中实现的，只能由实现接口的类来实现接口中的方法。
