---
title: Java枚举
date: 2021-11-28
tags: Java
author: 映雪
---

山海自有归期，风雨自有相逢。

<!--more-->

### 枚举

- 枚举是一个被命名的整型常数的集合，用于声明一组带标识符的常数。
- 类似当一个变量有几种固定可能的取值时，就可以将它定义为枚举类型。例如一个人的性别只能是“男”或者“女”，一周的星期只能是 7 天中的一个等。

```java
  enum Color{
    red,greed,blue
  }
```

### 枚举使用

```java
public class EnumerationTester {
  enum Color{
    red,greed,blue
  }
  public static void main(String args[]){
    Color blue = Color.blue;
    System.out.println(blue);
  }
}

```

- 枚举 Color 转化

```java
class Color
{
     public static final Color RED = new Color();
     public static final Color BLUE = new Color();
     public static final Color GREEN = new Color();
}
```

### 枚举类成员

- 枚举跟普通类一样可以用自己的变量、方法和构造函数，构造函数只能使用 private 访问修饰符，所以外部无法调用。
- 枚举既可以包含具体方法，也可以包含抽象方法。 如果枚举类具有抽象方法，则枚举类的每个实例都必须实现它。

```java
enum Color{
    RED{
        public String getColor(){//枚举对象实现抽象方法
            return "红色";
        }
    },
    GREEN{
        public String getColor(){//枚举对象实现抽象方法
            return "绿色";
        }
    },
    BLUE{
        public String getColor(){//枚举对象实现抽象方法
            return "蓝色";
        }
    };
    public abstract String getColor();//定义抽象方法
}

public class Test{
    public static void main(String[] args) {
        for (Color c:Color.values()){
            System.out.print(c.getColor() + "、");
        }
    }
}
```

### 枚举方法

1. values() 返回枚举类中所有的值。
2. ordinal()方法可以找到每个枚举常量的索引，就像数组索引一样。
3. valueOf()方法返回指定字符串值的枚举常量。

```java
enum Color
{
    RED, GREEN, BLUE;
}

public class Test
{
    public static void main(String[] args)
    {
        // 调用 values()
        Color[] arr = Color.values();

        // 迭代枚举
        for (Color col : arr)
        {
            // 查看索引
            System.out.println(col + " at index " + col.ordinal());
        }

        // 使用 valueOf() 返回枚举常量，不存在的会报错 IllegalArgumentException
        System.out.println(Color.valueOf("RED"));
        // System.out.println(Color.valueOf("WHITE"));
    }
}
```
