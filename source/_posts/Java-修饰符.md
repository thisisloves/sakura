---
title: Java修饰符
date: 2020-11-25
tags: Java
author: 映雪
---

等你跟自己和解了，对的人自然就出现了。你连跟自己都相处不下去，世上便无对的人。

<!--more-->

## Java 修饰符

1. 访问修饰符
2. 非访问修饰符

```java
public String name;
private boolean myFlag;
static final double weeks = 9.5;
protected static final int BOXWIDTH = 42;

```

### 访问控制修饰符

- Java 中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

1. **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。
2. **private** : 在同一类内可见（不包含子孙类）。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
3. **public** : 对所有类可见。使用对象：类、接口、变量、方法。
4. **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。

| 访问级别 | 访问控制修饰符 | 同类 |  同包  | 子类(不同包) | 不同包(其他类) |
| :------: | :------------: | :--: | :----: | :----------: | :------------: |
|   公共   |     public     | 允许 |  允许  |     允许     |      允许      |
|  受保护  |   protected    | 允许 |  允许  |     允许     |     不允许     |
|   默认   |   缺省修饰符   | 允许 |  允许  |    不允许    |     不允许     |
|   私有   |    private     | 允许 | 不允许 |    不允许    |     不允许     |

### 非访问修饰符

1. **static 修饰符**，用来修饰类方法和类变量。

（1).静态变量

- static 关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。 静态变量也被称为类变量。局部变量不能被声明为 static 变量。

（2).静态方法

- static 关键字用来声明独立于对象的静态方法。静态方法不能使用类的非静态变量。静态方法从参数列表得到数据，然后计算这些数据。

```java
public class InstanceCounter {
   private static int numInstances = 0;
   protected static int getCount() {
      return numInstances;
   }
   private static void addInstance() {
      numInstances++;
   }

   InstanceCounter() {
      InstanceCounter.addInstance();
   }

   public static void main(String[] arguments) {
      System.out.println("Starting with " +
      InstanceCounter.getCount() + " instances");
      for (int i = 0; i < 500; ++i){
         new InstanceCounter();
          }
      System.out.println("Created " +
      InstanceCounter.getCount() + " instances");
   }
}
```

2. **final 修饰符**，用来修饰类、方法和变量，final 修饰的类不能够被继承，修饰的方法不能被继承类重新定义，修饰的变量为常量，是不可修改的。

（1).final 变量

- final 表示"最后的、最终的"含义，变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定 初始值。
- final 修饰符通常和 static 修饰符一起使用来创建类常量。

（2).final 方法

- 父类中的 final 方法可以被子类继承，但是不能被子类重写。
- 声明 final 方法的主要目的是防止该方法的内容被修改。

```java
public class Test{
  final int value = 10;
  // 下面是声明常量的实例
  public static final int BOXWIDTH = 6;
  static final String TITLE = "Manager";

  public void changeValue(){
     value = 12; //将输出一个错误
  }
   public final void changeName(){
       // 方法体
    }
}
```

3. **abstract 修饰符**，用来创建抽象类和抽象方法。

（1).abstract 类

- 抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。
- 一个类不能同时被 abstract 和 final 修饰。如果一个类包含抽象方法，那么该类一定要声明为抽象类，否则将出现编译错误。
- 抽象类可以包含抽象方法和非抽象方法。

（2).abstract 方法

- 抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。
- 抽象方法不能被声明成 final 和 static。
- 任何继承抽象类的子类必须实现父类的所有抽象方法，除非该子类也是抽象类。
- 如果一个类包含若干个抽象方法，那么该类必须声明为抽象类。抽象类可以不包含抽象方法。
- 抽象方法的声明以分号结尾，例如：**public abstract sample();**。

```java
abstract class Caravan{
   private double price;
   private String model;
   private String year;
   public abstract void goFast(); //抽象方法
   public abstract void changeColor();
}
```

4. **synchronized 修饰符** 和 **volatile 修饰符**，主要用于线程的编程。

- synchronized 关键字声明的方法同一时间只能被一个线程访问。synchronized 修饰符可以应用于四个访问修饰符。

```java
public synchronized void showDetails(){
.......
}
```

- volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。- 一个 volatile 对象引用可能是 null。

```java
public class MyRunnable implements Runnable
{
    private volatile boolean active;
    public void run()
    {
        active = true;
        while (active) // 第一行
        {
            // 代码
        }
    }
    public void stop()
    {
        active = false; // 第二行
    }
}
```

5. **transient 修饰符**

- 序列化的对象包含被 transient 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。
- 该修饰符包含在定义变量的语句中，用来预处理类和变量的数据类型。

```java
public transient int limit = 55;   // 不会持久化
public int b; // 持久化
```
