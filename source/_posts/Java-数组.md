---
title: Java数组
date: 2021-1-24
tags: Java
author: 映雪
---

水遥山远谩相思，情知难舍弃，何似莫分飞。

<!--more-->

### 数组声明

- 语法

```
dataType[] arrayRefVar ;  // 首选方法
dataType arrayRefVar [];  // 效果相同，但不是首选方法
```

- 实例

```java
double [] myList; // 首选方法
double myList []; // 效果相同，但不是首选方法
```

### 创建数组

- java 使用 new 操作符来创建数组

```
arrayRefVar = new dataType[arraySize];

```

- 上面的语法语句做了两件事

1. 使用 dataType[arraySize] 创建了一个数组。
2. 把新创建的数组的引用赋值给变量 arrayRefVar。

- 数组变量的声明，和创建数组可以用一条语句完成，如下所示：

```
dataType[] arrayRefVar = new dataType[arraySize];
```

- 还可以使用如下的方式创建数组

```
dataType[] arrayRefVar = {value0, value1, ..., valuek};
```

- 数组的元素是通过索引访问的。数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1。

- 实例

```java
public class TestArray {
  public static void main(String[] args) {
    // 数组大小
    int size = 10;
    double [] myList = new double[size];
    myList[0] = 5.6;
    myList[1] = 4.4;
    myList[2] = 13.2;
    myList[3] = 5.6;
    myList[4] = 6.6;
    myList[5] = 7.7;
    myList[6] = 8.8;
    myList[9] = 9.0;
    myList[7] = 4.0;
    myList[8] = 4.2;

    // 计算所有元素的总和
    double total = 0;
    for (int i=0;i < myList.length;i++){
      total =  myList[i] + total;
    }
    System.out.println("数组的总和为"+total);
  }
}

```

### 处理数组

- 数组循环

```java
public class TestArray {
   public static void main(String[] args) {
      double[] myList = {1.9, 2.9, 3.4, 3.5};

      // 打印所有数组元素
      for (int i = 0; i < myList.length; i++) {
         System.out.println(myList[i] + " ");
      }
      // 计算所有元素的总和
      double total = 0;
      for (int i = 0; i < myList.length; i++) {
         total += myList[i];
      }
      System.out.println("Total is " + total);
      // 查找最大元素
      double max = myList[0];
      for (int i = 1; i < myList.length; i++) {
         if (myList[i] > max) max = myList[i];
      }
      System.out.println("Max is " + max);
   }
}
```

- 数组作为函数的参数

```java
public static void printArray(int[] array) {
  for(int i = 0; i<array.length;i++){
    System.out.print(array[i]+"");
  }
}
// 调用printArray 方法打印
printArray(new int[]{3,1,4,5,6,6})
```

- 数组可以作为参数传递给方法。

```java

// 数组作为函数的返回值
public static int[] reverse(int[] list) {
  int[] result = new int[list.length];

  for (int i = 0, j = result.length - 1; i < list.length; i++, j--) {
    result[j] = list[i];
  }
  return result;
}
```

### Arrays 类

- java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。

1. **fill()**

```java
public static void fill(int[] a, int val)
```

- 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。

2. **sort()**

```java
public static void sort(Object[] a)
```

- 对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。

3. **equals()**

```java
public static boolean equals(long[] a, long[] a2)
```

-  如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int 等）。

4. **binarySearch()**

```java
public static int binarySearch(Object[] a, Object key) 
```

-  用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double 等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点)

