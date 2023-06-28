---
title: String类
date: 2021-1-5
tags: Java
author: 映雪
---

可惜一溪风月，莫教踏碎琼瑶。

<!--more-->

### String 类

- 字符串广泛应用 在 Java 编程中，在 Java 中字符串属于对象，Java 提供了 String 类来创建和操作字符串。

### 字符串创建

1. 普通创建

```java
String str = "Runoob";
```

- 在代码中遇到字符串常量时，这里的值是 "Runoob""，编译器会使用该值创建一个 String 对象。
- 和其它对象一样，可以使用关键字和构造方法来创建 String 对象。

2. 构造函数创建

```java
String str2= new String("Runoob");
```

- RuString 创建的字符串存储在公共池中，而 new 创建的字符串对象在堆上

```java
String s1 = "Runoob";              // String 直接创建
String s2 = "Runoob";              // String 直接创建
String s3 = s1;                    // 相同引用
String s4 = new String("Runoob");   // String 对象创建
String s5 = new String("Runoob");   // String 对象创建

```

![截屏2021-08-16 下午4.41.45.png](/images/2021/08/16/Xk5A1la48zD7qPw.png)

```java
public class Stringdemo{
    public static void main(String args[]){
        char [] helloArray = {'r','u','n','o','o','b'};
        String helloString = new String(helloArray);
        System.out.println(helloString)
    }
}
```

- 结果

```
runoob
```

- **注意**:String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了
- 如果需要对字符串做很多修改，那么应该选择使用 StringBuffer & StringBuilder 类。

### 字符串长度

- 用于获取有关对象的信息的方法称为访问器方法。
- String 类的一个访问器方法是 length() 方法，它返回字符串对象包含的字符数。

```java
public class StringDemo {
    public static void main(String args[]) {
        String site = "www.zzzzzz.com";
        int len = site.length();
        System.out.println( "长度 : " + len );
   }
}
```

### 连接字符串

1. **concat()**

```
string1.concat(string2);
```

- 返回 string2 连接 string1 的新字符串。

2. 使用**加号**操作符来连接字符串

```
"Hello," + " runoob" + "!"
```

- 结果

```
"Hello, runoob!"
```

```java
public class StringDemo {
    public static void main(String args[]) {
        String string1 = "网址：";
        System.out.println("1、" + string1 + "www.xxxxxcom");
    }
}
```

### 创建格式化字符串

- 输出格式化数字可以使用 printf() 和 format() 方法。
- String 类使用静态方法 format() 返回一个 String 对象而不是 PrintStream 对象。
- String 类的静态方法 format() 能用来创建可复用的格式化字符串，而不仅仅是用于一次打印输出。

```java
System.out.printf("浮点型变量的值为 " +
"%f, 整型变量的值为 " +
" %d, 字符串变量的值为 " +
"is %s", floatVar, intVar, stringVar);
```

```java
String fs;
fs = String.format("浮点型变量的值为 " +
"%f, 整型变量的值为 " +
" %d, 字符串变量的值为 " +
" %s", floatVar, intVar, stringVar);
```

### String 类方法

| 方法                                                         |                                       描述                                       |
| ------------------------------------------------------------ | :------------------------------------------------------------------------------: |
| char charAt(int index)                                       |                            返回指定索引处的 char 值。                            |
| int compareTo(Object o)                                      |                          把这个字符串和另一个对象比较。                          |
| int compareTo(String anotherString)                          |                            按字典顺序比较两个字符串。                            |
| int compareToIgnoreCase(String str)                          |                     按字典顺序比较两个字符串，不考虑大小写。                     |
| String concat(String str)                                    |                        将指定字符串连接到此字符串的结尾。                        |
| boolean contentEquals(StringBuffer sb)                       |             仅当字符串与指定的 StringBuffer 有相同顺序的字符返回真。             |
| static String copyValueOf(char[] data)                       |                     返回指定数组中表示该字符序列的 String。                      |
| static String copyValueOf(char[] data,int offset, int count) |                     返回指定数组中表示该字符序列的 String。                      |
| boolean endsWith(String suffix)                              |                        测试该字符串是否以指定的后缀结束。                        |
| boolean equals (Objectc anObject)                            |                           将此字符串与指定的对象比较。                           |
| boolean equalsIgnoreCase(String anotherString)               |                 将此 String 与另一个 String 比较，不考虑大小写。                 |
| byte[] getBytes()                                            |       使用默认字符集将 String 编码为 byte 序列，并存储到新的 byte 数组中。       |
| byte[] getBytes(String charsetName)                          |     使用指定的字符集将此 String 编码为 byte 序列，并储存到新的 byte 数组中。     |
| int hashCode()                                               |                              返回此字符串的哈希码。                              |
| int indexOf(int ch)                                          |                   返回指定字符在此字符串中第一次出现处的索引。                   |
| int indexOf(int ch, int fromIndex)                           |        返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。        |
| int indexOf(String str)                                      |                 返回指定子字符串在此字符串中第一次出现处的索引。                 |
| int indexOf(String str, int fromIndex)                       |        返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。        |
| String intern()                                              |                         返回字符串对象的规范化表示形式。                         |
| int lastIndexOf(int ch)                                      |                  返回指定字符在此字符串中最后一次出现处的索引。                  |
| int lastIndexOf(int ch, int fromIndex)                       |  返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。  |
| int lastIndexOf(String str)                                  |                 返回指定子字符串在此字符串中最右边出现处的索引。                 |
| int lastIndexOf(String str, int fromIndex)                   |   返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。   |
| int length()                                                 |                               返回此字符串的长度。                               |
| boolean matches(String regex)                                |                      告知此字符串是否匹配给定的正则表达式。                      |
| String replace(char oldChar, char newChar)                   | 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 |
| String replaceAll(String regex, String replacement)          |     使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。      |
| String replace(char oldChar, char newChar)                   | 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 |
| String replaceAll(String regex, String replacement)          |     使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。      |
| String replaceFirst(String regex, String replacement)        |    使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。     |
| String[] split(String regex)                                 |                      根据给定正则表达式的匹配拆分此字符串。                      |
| String[] split(String regex, int limit)                      |                     根据匹配给定的正则表达式来拆分此字符串。                     |
| boolean startsWith(String prefix)                            |                        测试此字符串是否以指定的前缀开始。                        |
| boolean startsWith(String prefix, int toffset)               |             测试此字符串从指定索引开始的子字符串是否以指定前缀开始。             |
| CharSequence subSequence(int beginIndex, int endIndex)       |                  返回一个新的字符序列，它是此序列的一个子序列。                  |
| String substring(int beginIndex)                             |                 返回一个新的字符串，它是此字符串的一个子字符串。                 |
| String substring(int beginIndex, int endIndex)               |                  返回一个新字符串，它是此字符串的一个子字符串。                  |
| char[] toCharArray()                                         |                        将此字符串转换为一个新的字符数组。                        |
| String toLowerCase()                                         |           使用默认语言环境的规则将此 String 中的所有字符都转换为小写。           |
| String toLowerCase(Locale locale)                            |           使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。           |
| String toString()                                            |                     返回此对象本身（它已经是一个字符串！）。                     |
| String toUpperCase()                                         |           使用默认语言环境的规则将此 String 中的所有字符都转换为大写。           |
| String toUpperCase(Locale locale)                            |           使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。           |
| String trim()                                                |                    返回字符串的副本，忽略前导空白和尾部空白。                    |
| static String valueOf(primitive data type x)                 |                 返回给定 data type 类型 x 参数的字符串表示形式。                 |
| contains(CharSequence chars)                                 |                           判断是否包含指定的字符系列。                           |
| isEmpty()                                                    |                               判断字符串是否为空。                               |
