---
title: Java日期
date: 2021-2-15
tags: Java
author: 映雪
---

红叶有霜终日醉,醉到深处是相思。

<!--more-->

### Java 日期

- java.util 包提供了 Date 类来封装当前的日期和时间。 Date 类提供两个构造函数来实例化 Date 对象。

- 第一个构造函数使用当前日期和时间来初始化对象。

```
Date( )
```

- 第二个构造函数接收一个参数，该参数是从 1970 年 1 月 1 日起的毫秒数。

```
Date(long millisec)
```

### 日期对象方法

|             方法              |                                         描述                                         |
| :---------------------------: | :----------------------------------------------------------------------------------: |
|   boolean after(Date date)    |          若当调用此方法的 Date 对象在指定日期之后返回 true,否则返回 false。          |
|  booleean before(Date date)   |          若当调用此方法的 Date 对象在指定日期之前返回 true,否则返回 false。          |
|        Object clone()         |                                  返回此对象的副本。                                  |
|   int compareTo(Date date)    |                       比较当调用此方法的 Date 对象和指定日期。                       |
|   int compareTo(Date date)    | 若 obj 是 Date 类型则操作等同于 compareTo(Date) 。否则它抛出 ClassCastException。|
| booleam equals (Object date ) |         当调用此方法的 Date 对象和指定日期相等的时候返回 true 否则返回 false         |
|        long getTime()         |         返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。         |
|         int hashCode          |                                返回此对象的哈希码值。                                |
|    void setTime(long time)    |         用自 1970 年 1 月 1 日 00:00:00 GMT 以后 time 毫秒数设置时间和日期。         |
|       String toString()       |                  转换 Date 对象为 String 表示形式，并返回该字符串。                  |


### 获取当前日期时间

- Java 中获取当前日期和时间很简单，使用 Date 对象的 toString() 方法来打印当前日期和时间

```java
public class DateDemo {
public static void main(String args[]) {
// 初始化 Date 对象
Date date = new Date();

   // 使用 toString() 函数显示日期时间
   System.out.println(date.toString());
}
```

### 日期比较

- Java 使用以下三种方法来比较两个日期

1. **getTime()** 方法获取两个日期（自 1970 年 1 月 1 日经历的毫秒数值），然后比较这两个值。
2. **before()，after() 和 equals()**。例如，一个月的 12 号比 18 号早，则 new Date(99, 2, 12).before(new Date (99, 2, 18)) 返回 true。
3. **compareTo()**  是由 Comparable 接口定义的，Date 类实现了这个接口。


### 格式化日期

1.**SimpleDateFormat**

- SimpleDateFormat 是一个以语言环境敏感的方式来格式化和分析日期的类。SimpleDateFormat 允许你选择任何用户自定义日期时间格式来运行。例如：

```java
public class dateDemo {
       public static void main(String args[]){
              Date dNow = new Date();
              SimpleDateFormat ft  = new SimpleDateFormat ("yyyy-MM-dd hh:mm:ss");
              System.out.println("当前时间为:"+ft.format(dNow));
       }
}

```

2. **printf**

- printf方法可以很轻松地格式化时间和日期。使用两个字母格式，它以t开头并且以下面表格中的一个字母结尾。

```java
import java.util.Date;

public class DateDemo {

  public static void main(String args[]) {
     // 初始化 Date 对象
     Date date = new Date();

     // 使用toString()显示日期和时间
     String str = String.format("Current Date/Time : %tc", date );

     System.out.printf(str);
  }
}
```