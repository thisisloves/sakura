---
title: Math类和Number类
date: 2020-12-15
tags: Java
author: 映雪
---

这世界上有一千种等待，最好的那种叫做来日可期，我愿意从这一秒开始倒数等待多年后的相遇。

<!--more-->

### Number 类

- 一般情况下使用数据的基本数据类型：byte、int、short、long、double、float、boolean、char；
- 对应的包装类型也有八种：Byte、Integer、Short、Long、Double、Float、Character、Boolean；
- 包装类型都是用 final 声明了，不可以被继承重写；在实际情况中编译器会自动的将基本数据类型装箱成对象类型，或者将对象类型拆箱成基本数据类型；

```java
public static void main(String[] args) {
	int num1 = 1;
	//将基本数据类型装箱成对象包装类型
	Integer num2 = num1;
	Integer num3 = 3;
	//将对象数据类拆箱
	int num4 = num3;
}
```

- Number 类是 java.lang 包下的一个抽象类，提供了将包装类型拆箱成基本类型的方法，所有基本类型（数据类型）的包装类型都继承了该抽象类，并且是 final 声明不可继承改变

```java
package java.lang;
public abstract class Number implements java.io.Serializable {
    public abstract int intValue();
    public abstract long longValue();
    public abstract float floatValue();
    public abstract double doubleValue();
    public byte byteValue() {
        return (byte)intValue();
    }
    public short shortValue() {
        return (short)intValue();
    }
    private static final long serialVersionUID = -8742448824652078965L;
}
```

### Number 类方法

|    方法     |                               描述                               |
| :---------: | :--------------------------------------------------------------: |
| xxxValue()  | xxxValue() 方法用于将 Number 对象转换为 xxx 数据类型的值并返回。 |
| compareTo() |                    将 number 对象与参数比较。                    |
|  equals()   |                 判断 number 对象是否与参数相等。                 |
|  valueOf()  |            返回一个 Integer 对象指定的内置数据类型。             |
| toString()  |                       以字符串形式返回值。                       |
| parseInt()  |                    将字符串解析为 int 类型。                     |

### Math 类

- Java 的 Math 类 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。
- Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。

```java
public class Test {
    public static void main (String []args)
    {
        System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));
        System.out.println("0度的余弦值：" + Math.cos(0));
        System.out.println("60度的正切值：" + Math.tan(Math.PI/3));
        System.out.println("1的反正切值： " + Math.atan(1));
        System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));
        System.out.println(Math.PI);
    }
}


```

### Math 类方法

|   方法   |                             描述                             |
| :------: | :----------------------------------------------------------: |
|  abs()   |                      返回参数的绝对值。                      |
|  ceil()  | 返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。 |
| floor()  |           返回小于等于（<=）给定参数的最大整数 。            |
| round()  |              返回一个最接近的 int、long 型值。               |
|  min()   |                   返回两个参数中的最小值。                   |
|  max()   |                   返回两个参数中的最大值。                   |
|  pow()   |               返回第一个参数的第二个参数次方。               |
|  sqrt()  |                     求参数的算术平方根。                     |
|  sin()   |               求指定 double 类型参数的正弦值。               |
|  cos()   |               求指定 double 类型参数的余弦值。               |
|  tan()   |               求指定 double 类型参数的正切值。               |
| random() |                       返回一个随机数。                       |
