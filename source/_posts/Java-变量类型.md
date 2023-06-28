---
title: Java变量类型
date: 2020-11-02
tags: Java
author: 映雪
---

有些人一旦错过了，就是一辈子不再主动联系，不愿打扰你的生活，连偶尔的寒暄都没有，成长就是这样的，不断的告别，不断的遇见。

<!--more-->

### Java 变量

- 变量是存储数据值的容器。
- **在 Java 中，所有的变量在使用前必须声明。**

### Java 变量创建

```
type identifier [ = value][, identifier [= value] ...] ;
```

- type 为 Java 数据类型。identifier 是变量名。可以使用逗号隔开来声明多个同类型变量。

### Java 变量类型

1. 类变量：独立于方法之外的变量，用 static 修饰。
2. 实例变量：独立于方法之外的变量，不过没有 static 修饰。
3. 局部变量：类的方法中的变量。

```java
public class Variable{
    static int allClicks=0;    // 类变量
    String str="hello world";  // 实例变量
    public void method(){
        int i =0;  // 局部变量
    }
}
```

### Java 实例变量

- 实例变量声明在一个类中，但在方法、构造方法和语句块之外。
- 当一个对象被实例化之后，每个实例变量的值就跟着确定。
- 实例变量在对象创建的时候创建，在对象被销毁的时候销毁。
- 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。

```java
import java.io.*;
public class Employee{
  // 这个实例变量对子类可见
  public String name;
  // 私有变量，仅在该类可见
  private double salary;
  //在构造器中对name赋值
  public Employee (String empName){   name = empName;   }
  //设定salary的值
  public void setSalary(double empSal){      salary = empSal;   }
  // 打印信息
  public void printEmp(){
    System.out.println("名字 : " + name );
    System.out.println("薪水 : " + salary);   }
  public static void main(String[] args){
    Employee empOne = new Employee("RUNOOB");
    empOne.setSalary(1000.0);
    empOne.printEmp();
  }
}
```

### Java 类变量（静态变量）

- 类变量也称为静态变量，在类中以 static 关键字声明，但必须在方法之外。
- 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
- 静态变量在第一次被访问时创建，在程序结束时销毁。
- 静态变量可以通过：ClassName.VariableName的方式访问。

```java
import java.io.*;

  public class Employee {
    //salary是静态的私有变量
    private static double salary;
    // DEPARTMENT是一个常量
    public static final String DEPARTMENT = "开发人员";
    public static void main(String[] args){
      salary = 10000;
      System.out.println(DEPARTMENT+"平均工资:"+salary);
    }
  }
```
