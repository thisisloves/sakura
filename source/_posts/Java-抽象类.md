---
title: Java抽象类
date: 2021-11-24
tags: Java
author: 映雪
---

落日归山海，烟火向星辰。

<!--more-->

### 抽象类

- 在 Java 面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。
- 抽象类除了不能实例化对象之外，类的其它功能依然存在，成员变量、成员方法和构造方法的访问方式和普通类一样。
- 由于抽象类不能实例化对象，所以抽象类必须被继承，才能被使用。

```java
public abstract class Employee_Abstract{
  private String name;
  private String address;
  private int number;
  public Employee_Abstract(String name,String address,int number){
    System.out.println("Employee_Abstract 的构造函数");
    this.name = name;
    this.address = address;
    this.number = number;
  }
  public double computePay(){
    System.out.println(" Employee_Abstract 的 computePay");
    return 0.0;
  }
  public void mailCheck(){
    System.out.println("Mailing a check to"+this.name+""+this.address);
  }
  public String toSting(){
    return name +""+address+""+number;
  }
  public String getName(){
    return name;
  }
  public String getAddress(){
    return address;
  }
  public void setAddress(String NewAddress){
    address = NewAddress;
  }
  public int getNumber(){
    return number;
  }
}

```

### 抽象类继承

```java
public class Abstract_Test {
  public static void main(String[] args) {
    Salary_Test s = new Salary_Test("cxm", "china", 18, 9.0);
    Employee_Abstract e = new Salary_Test("yhh","china",24,0.9);
    s.computePay();
    e.computePay();
    e.mailCheck();
    s.mailCheck();
    System.out.println(s.getName());
    System.out.println(e.getName());
  }
}

class Salary_Test extends Employee_Abstract {
    private double salary;

    public Salary_Test(String name, String address, int number, double salary) {
      super(name, address, number);
      System.out.println("Salary_Test 类的构造函数");
      setSalary(salary);
    }

    public void mailCheck() {
      System.out.println("Salary_Test 类的方法");
      System.out.println("Mailing check to" + getName() + "with Salary_Test" + salary);
    }

    public void setSalary(double newSalary) {
      if (newSalary >= 0.0) {
        salary = newSalary;
      }
    }

    public double computePay() {
      System.out.println("Computing salary pay for" + getName());
      return salary / 52;
    }
  }

```

### 抽象方法

- Abstract 关键字可以用来声明抽象方法，抽象方法只包含一个方法名，而没有方法体。
- 抽象方法没有定义，方法名后面直接跟一个分号，而不是花括号。

```java
public abstract class Employee
{
   private String name;
   private String address;
   private int number;

   public abstract double computePay();

}
```

- 如果一个类包含抽象方法，那么该类必须是抽象类。
- 任何子类必须重写父类的抽象方法，或者声明自身为抽象类。
- 继承抽象方法的子类必须重写该方法。否则，该子类也必须声明为抽象类。最终，必须有子类实现该抽象方法，否则，从最初的父类到最终的子类都不能用来实例化对象。

```java
public class Salary extends Employee
{
   private double salary; // Annual salary
   public double computePay()
   {
      System.out.println("Computing salary pay for " + getName());
      return salary/52;
   }

}
```

### 抽象类总结

1. **抽象类不能被实例化**，如果被实例化，就会报错，编译无法通过。只有抽象类的非抽象子类可以创建对象。
2. 抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类。
3. 抽象类中的抽象方法只是声明，不包含方法体，就是不给出方法的具体实现也就是方法的具体功能。
4. 构造方法，类方法（用 static 修饰的方法）不能声明为抽象方法。
5. 抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类。
