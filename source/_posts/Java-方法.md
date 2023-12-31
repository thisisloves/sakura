---
title: Java方法
date: 2020-11-23
tags: Java
author: 映雪
---

清风湿润,茶烟轻扬。重温旧梦,故人已去。

<!--more-->

### 举例

- 经常使用到 System.out.println()，那么它是什么呢？

* `println()` 是一个方法。
* `System` 是系统类。
* `out` 是标准输出对象。
* 这句话的用法是调用系统类 System 中的标准输出对象 out 中的方法 println()。

### Java 方法

1. 方法是解决一类问题的步骤的有序组合。
2. 方法包含于类或对象中。
3. 方法在程序中被创建，可在其他地方被引用。

### 方法的优点

1. 使程序变得更简短而清晰。
2. 有利于程序维护。
3. 可以提高程序开发的效率。
4. 提高了代码的重用性。

### 方法的命名规则

1. 方法的名字的第一个单词应以小写字母作为开头，后面的单词则用大写字母开头写，不使用连接符。例如：addPerson。
2. 下划线可能出现在 JUnit 测试方法名称中用以分隔名称的逻辑组件。一个典型的模式是：test<MethodUnderTest>\_<state>，例如 testPop_emptyStack。

### 方法的定义

![截屏2021-10-14 上午10.27.31.png](/images/2021/10/14/UnFNYit8EIOGWj4.png)

1. **修饰符**：修饰符，这是可选的，告诉编译器如何调用该方法。定义了该方法的访问类型。
2. **返回值类型** ：方法可能会返回值。returnValueType 是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType 是关键字 void。
3. **方法名**：是方法的实际名称。方法名和参数表共同构成方法签名。
4. **参数类型**：参数像是一个占位符。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。
5. **方法体**：方法体包含具体的语句，定义该方法的功能。

```java
public class TestMax {
   /** 主方法 */
   public static void main(String[] args) {
      int i = 5;
      int j = 2;
      int k = max(i, j);
      System.out.println( i + " 和 " + j + " 比较，最大值是：" + k);
   }

   /** 返回两个整数变量较大的值 */
   public static int max(int num1, int num2) {
      int result;
      if (num1 > num2)
         result = num1;
      else
         result = num2;

      return result;
   }
}
```

- **注意**： void 在方法声明的时候表示该方法没有返回值，无返回值,但可以在方法里用 return 来退出方法。
