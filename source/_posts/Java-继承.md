---
title: Java继承
date: 2021-10-12
tags: Java
author: 映雪
---

东边日出西边雨，道是无晴却有晴。

<!--more-->

### 继承

- 继承是 Java 面向对象编程技术的一块基石，因为它允许创建分等级层次的类。
- 继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为

### 继承类型

- Java 不支持多继承，但支持多重继承。

![截屏2021-12-17 下午6.24.50.png](/images/2021/12/17/VmsqYMAWtnurg1Z.png)

### 继承的特性

1. 子类拥有父类非 private 的属性、方法。
2. 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
3. 子类可以用自己的方式实现父类的方法。

- Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如 B 类继承 A 类，C 类继承 B 类，所以按照关系就是 B 类是 C 类的父类，A 类是 B 类的父类，这是 Java 继承区别于 C++ 继承的一个特性。

- 提高了类之间的耦合性（继承的缺点，耦合度高就会造成代码之间的联系越紧密，代码独立性越差）。

### extends 关键字

- 在 Java 中，类的继承是单一继承，也就是说，**一个子类只能拥有一个父类**，所以 extends 只能继承一个类。

```java
class Animal {
  private int id;
  private String name;

  public void eat(int id,String name)
  {
    System.out.println(name+"吃东西");
  }
  public void sleep(int id,String name){
    System.out.println(name+"睡觉");
  }
  public void introduce (int id,String name){
    System.out.println("大家好，我叫"+name+"我的id是"+id);
  }

 class Penguin extends Animal{
 public void penguin(int id,String name){
    super.eat(id,name);
    super.sleep(id,name);
    super.introduce(id,name);
  }
}
public class Test_Extends {
  public static void main(String args[]){
    Animal a = new Animal();
    Penguin c = new Penguin();
    a.eat(1,"3");
    c.penguin(3,"猫咪");
  }
}

```

### implements 关键字

- implements 关键字可以变相的使 Java 具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。

```java

 // 接口是抽象方法和常量值定义的集合，是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现
 // 接口通过使用关键字interface来声明
 // 接口只关心功能，并不关心功能的具体实现，使用相同接口的类不一定有继承关系

 interface Animal2 {
   // 接口中只包含常量和方法的定义，没有变量和方法的实现，必须重写才能使用
   // 接口中成员的访问类型都是public
   // 抽象方法的好处是允许方法的定义和实现分开
    String name = "小狗";
    void eat(String name);
    void sleep(String name);
 }
interface Animal3{
  public String love ="抓猫";
  public String name = "小猫";
  public Object active(String name, String love);
}
 // 一个类不能实现该接口的所有抽象方法，那么这个类必须被定义为抽象方法。
 // 抽象类是需要被继承的，抽象方法是需要被重写的，建议重写抽象类中的所有方法（包含非抽象方法）
 // 抽象类不能被实例化
 // 可不包含抽象方法
 // 抽象类有构造方法，有父类的，也遵循单继承的规律
abstract class Tiger implements Animal2,Animal3{
  public String name ="老虎";
  @Override
  public void eat(String name){
    System.out.println(name+"吃东西");
  }
  @Override
    public void sleep(String name){
     System.out.println(name+"睡觉");
  }
  @Override
    public Object active(String name, String love){
    System.out.println(name+"喜欢"+love);
    return null;
  }
  public void say(String name){
    System.out.println("我是"+name);
  }
}

class Tiger2 implements Animal2,Animal3{
   public String name ="老虎";
   @Override
   public void eat(String name){
     System.out.println(name+"吃东西");
   }
   @Override
   public void sleep(String name){
     System.out.println(name+"睡觉");
   }
   @Override
   public Object active(String name, String love){
     System.out.println(name+"喜欢"+love);
     return null;
   }
   public void say(String name){
     System.out.println("我是"+name);
   }
 }

class TigerSon extends Tiger{
  public String name ="老虎儿子";

  public Object active (){
   return super.active(name,love);
  }
  public void sleep(){
    super.sleep(name);
  }
  public void getName() {
    System.out.println("父类Name" +super.name+",子类Name"+this.name );
  }
}
 public class Implements_Test {
   public static void main(String args[]){
     System.out.println("接口Animal2的Name"+ Animal2.name);
     System.out.println("接口Animal3的Name"+ Animal3.name+",接口Animal3的Love"+Animal3.love);
     TigerSon tigerSon = new TigerSon();
     tigerSon.active();
     tigerSon.active(tigerSon.name,"吃东西");

     tigerSon.sleep();
     tigerSon.sleep("老虎");

     tigerSon.getName();
   }
 }

```

### super 与 this 关键字

- super 关键字：我们可以通过 super 关键字来实现对父类成员的访问，用来引用当前对象的父类。
- this 关键字：指向自己的引用。

```java
class Animal {
  private int id;
  private String name;

  public void eat(int id,String name)
  {
    System.out.println(name+"吃东西");
  }
  public void sleep(int id,String name){
    System.out.println(name+"睡觉");
  }
  public void introduce (int id,String name){
    System.out.println("大家好，我叫"+name+"我的id是"+id);
  }

class Dog extends Animal{
  Dog(int id,String name){ // 构造函数
    super.eat(id,name);
    super.sleep(id,name);
    super.introduce(id,name); // 调用父类方法
    this.eat();  // 调用自己类的方法
  }
  public void eat(){
    System.out.println("狗狗吃");
  }
}

public class Test_Extends {
  public static void main(String args[]){
      Dog b = new Dog(4,"狗狗");
      b.eat(2,"22");
  }
}

```

### new

- new 运算符后面是对构造函数的调用。
- new 运算符通过分配堆上的内存来创建类的实例。

### 构造器

- 子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 super 关键字调用父类的构造器并配以适当的参数列表。

- 如果父类构造器没有参数，则在子类的构造器中不需要使用 super 关键字调用父类构造器，系统会自动调用父类的无参构造器。

```java
class SuperClass {
  private int n;
  SuperClass(){  // 构造函数
    System.out.println("SuperClass()");
  }
  SuperClass(int n) {
    System.out.println("SuperClass(int n)");
    this.n = n;
  }
}
// SubClass 类继承
class SubClass extends SuperClass{
  private int n;
  SubClass(){ // 自动调用父类的无参数构造器
    System.out.println("SubClass");
  }

  public SubClass(int n){
    super(300);  // 调用父类中带有参数的构造器
    System.out.println("SubClass(int n):"+n);
    this.n = n;
  }
}
// SubClass2 类继承
class SubClass2 extends SuperClass{
  private int n;

  SubClass2(){
    super(300);  // 调用父类中带有参数的构造器
    System.out.println("SubClass2");
  }

  public SubClass2(int n){ // 自动调用父类的无参数构造器
    System.out.println("SubClass2(int n):"+n);
    this.n = n;
  }
}
public class TestSuperSub{
  public static void main (String args[]){
    System.out.println("------SubClass 类继承------");
    SubClass sc1 = new SubClass();
    SubClass sc2 = new SubClass(100);
    System.out.println("------SubClass2 类继承------");
    SubClass2 sc3 = new SubClass2();
    SubClass2 sc4 = new SubClass2(200);
  }
}

```

### 多重继承

```java

public class Test_Extends2 {
  public static void main(String args[]){
    // 静态方法
    Father.work("程序员");
    System.out.println("父类的静态变量"+Father.active);

    // 孙子
    System.out.println("孙子");
    Grandson grandson = new Grandson();
    grandson.Grandson();
    grandson.Grandson("孙子1",1);
    //  grandson.name; 不可访问
    System.out.println("孙子调用爷爷类");
    grandson.Father();
    grandson.game("玩泥巴");
    System.out.println("孙子调用父类");
    grandson.Son();
    grandson.Son("孙子",3);
    grandson.talk();

    // 儿子
    System.out.println("儿子");
    Son son = new Son();
    son.Son();
    son.Son("儿子",24);

    // 孙子
    System.out.println("爸爸");
    Father  father = new Father();
    father.game("抽烟");
  }
}

class Father{
  private int age = 30;
  public String name = "爸爸";
  protected  String like ="游泳";
  static String active ="开飞机";

  public void Father(){
    System.out.println("我是"+name+"我的年龄为"+age+",我是儿子的构造函数");
  }
  public void game (String like){
    System.out.println("我喜欢的运动是"+like);
  }
  public static void work(String active){
    System.out.println("我的工作是"+active);
  }
}
class Son extends Father{
  private int age = 24;
  private String name = "儿子";

   public void Son(){
     System.out.println("我是"+name+"我的年龄为"+age+",我是儿子的构造函数");
   }
   public void Son (String name,int age ){
     System.out.println("我是"+name+"我的年龄为"+age+",我重写了儿子的构造函数");
   }
   public void talk (){
     System.out.println("我谈论了");
   }
}

class Grandson extends  Son{
  private int age = 10;
  private String name = "孙子";

  public void Grandson(){
    System.out.println("我是"+name+"我的年龄为"+age+",我是孙子的构造函数");
  }
  public void Grandson (String name,int age ){
    System.out.println("我是"+name+"我的年龄为"+age+",我重写了孙子的构造函数");
  }
}

```
