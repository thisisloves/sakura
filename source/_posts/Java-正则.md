---
title: Java正则
date: 2021-3-15
tags: Java
author: 映雪
---

曾是你扰我清梦一场,曾是你予我半世凄凉。

<!--more-->

### 正则表达式

- 正则表达式定义了字符串的模式。
- 正则表达式可以用来搜索、编辑或处理文本。
- 正则表达式并不仅限于某一种语言，但是在每种语言中有细微的差别。

### Java 正则

- java.util.regex 包主要包括以下三个类

1. **Pattern 类**

- pattern 对象是一个正则表达式的编译表示。Pattern 类没有公共构造方法。要创建一个 Pattern 对象，你必须首先调用其公共静态编译方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数。

2. **Matcher 类**

- Matcher 对象是对输入字符串进行解释和匹配操作的引擎。与 Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象。

3. **PatternSyntaxException 类**

- PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误。

### 捕获组

- 捕获组是把多个字符当一个单独单元进行处理的方法，它通过对括号内的字符分组来创建。

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexMatches
{
    public static void main( String args[] ){

      // 按指定模式在字符串查找
      String line = "This order was placed for QT3000! OK?";
      String pattern = "(.*)(\\d+)(.*)";

      // 创建 Pattern 对象
      Pattern r = Pattern.compile(pattern);

      // 现在创建 matcher 对象
      Matcher m = r.matcher(line);
      if (m.find( )) {
         System.out.println("Found value: " + m.group(0) );
         System.out.println("Found value: " + m.group(1) );
         System.out.println("Found value: " + m.group(2) );
      } else {
         System.out.println("NO MATCH");
      }
   }
}
```

### 正则表达式语法

|   字符    |                                                     说明                                                     |
| :-------: | :----------------------------------------------------------------------------------------------------------: |
|    \      |                           将下一字符标记为特殊字符、文本、反向引用或八进制转义符。                           |
|     ^     |    匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，^ 还会与"\n"或"\r"之后的位置匹配。     |
|     $     |    匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，$ 还会与"\n"或"\r"之前的位置匹配。     |
|    \*     |                零次或多次匹配前面的字符或子表达式。例如，zo* 匹配"z"和"zoo"。* 等效于 {0,}。                 |
|     +     |       一次或多次匹配前面的字符或子表达式。例如，"zo+"与"zo"和"zoo"匹配，但与"z"不匹配。+ 等效于 {1,}。       |
|     ?     |        零次或一次匹配前面的字符或子表达式。例如，"do(es)?"匹配"do"或"does"中的"do"。? 等效于 {0,1}。         |
|    {n}    |          n 是非负整数。正好匹配 n 次。例如，"o{2}"与"Bob"中的"o"不匹配，但与"food"中的两个"o"匹配。          |
|   {n,}    |                                        n 是非负整数。至少匹配 n 次。                                         |
|   {n,m}   |                          m 和 n 是非负整数，其中 n <= m。匹配至少 n 次，至多 m 次。                          |
|     ?     | 当此字符紧随任何其他限定符之后时，匹配模式是"非贪心的"。而默认的"贪心的"模式匹配搜索到的、尽可能长的字符串。 |
|     .     |       匹配除"\r\n"之外的任何单个字符。若要匹配包括"\r\n"在内的任意字符，请使用诸如"[\s\S]"之类的模式。       |
| (pattern) |                                    匹配 pattern 并捕获该匹配的子表达式。                                     |
|  x ｜ y   |                                            匹配 x 或 y。例如，'z                                             |
|   [xyz]   |                        字符集。匹配包含的任一字符。例如，"[abc]"匹配"plain"中的"a"。                         |
|  [^xyz]   |              反向字符集。匹配未包含的任何字符。例如，"[^abc]"匹配"plain"中"p"，"l"，"i"，"n"。               |
|   [a-z]   |             字符范围。匹配指定范围内的任何字符。例如，"[a-z]"匹配"a"到"z"范围内的任何小写字母。              |
|    \b     |        匹配一个字边界，即字与空格间的位置。例如，"er\b"匹配"never"中的"er"，但不匹配"verb"中的"er"。         |
|    \B     |                      非字边界匹配。"er\B"匹配"verb"中的"er"，但不匹配"never"中的"er"。                       |
|    \cx    |                                           匹配 x 指示的控制字符。                                            |
|    \d     |                                         数字字符匹配。等效于 [0-9]。                                         |
|    \D     |                                       非数字字符匹配。等效于 [^0-9]。                                        |
|    \f     |                                       换页符匹配。等效于 \x0c 和 \cL。                                       |
|    \n     |                                       换行符匹配。等效于 \x0a 和 \cJ。                                       |
|    \r     |                                     匹配一个回车符。等效于 \x0d 和 \cM。                                     |
|    \s     |                    匹配任何空白字符，包括空格、制表符、换页符等。与 [ \f\n\r\t\v] 等效。                     |
|    \S     |                                 匹配任何非空白字符。与 [^ \f\n\r\t\v] 等效。                                 |
|    \t     |                                      制表符匹配。与 \x09 和 \cI 等效。                                       |
|    \v     |                                    垂直制表符匹配。与 \x0b 和 \cK 等效。                                     |
|    \w     |                             匹配任何字类字符，包括下划线。与"[A-Za-z0-9_]"等效。                             |
|    \W     |                                与任何非单词字符匹配。与"[^a-za-z0-9_]"等效。                                 |
|    \xn    |                  匹配 n，此处的 n 是一个十六进制转义码。十六进制转义码必须正好是两位数长。                   |
|   \num    |                                     匹配 num，此处的 num 是一个正整数。                                      |
|    \n     |                                       标识一个八进制转义码或反向引用。                                       |
|    \nm    |                                       标识一个八进制转义码或反向引用。                                       |
|   \nml    |                  当 n 是八进制数 (0-3)，m 和 l 是八进制数 (0-7) 时，匹配八进制转义码 nml。                   |
|    \un    |            匹配 n，其中 n 是以四位十六进制数表示的 Unicode 字符。例如，\u00A9 匹配版权符号 (©)。             |

### Matche 类的方法

1. **索引方法**

- 索引方法提供了有用的索引值，精确表明输入字符串中在哪能找到匹配

|            方法             |                                  说明                                  |
| :-------------------------: | :--------------------------------------------------------------------: |
|     public int start()      |                        返回以前匹配的初始索引。                        |
| public int start(int group) |       返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引       |
|      public int end()       |                     返回最后匹配字符之后的偏移量。                     |
|  public int end(int group)  | 返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量。 |

2. **查找方法**

- 查找方法用来检查输入字符串并返回一个布尔值，表示是否找到该模式

|              方法               |                                      说明                                      |
| :-----------------------------: | :----------------------------------------------------------------------------: |
|   public boolean lookingAt()    |                  尝试将从区域开头开始的输入序列与该模式匹配。                  |
|      public boolean find()      |                 尝试查找与该模式匹配的输入序列的下一个子序列。                 |
| public boolean find(int start） | 重置此匹配器，然后尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列。 |
|    public boolean matches()     |                           尝试将整个区域与模式匹配。                           |

3. **替换方法**

- 替换方法是替换输入字符串里文本的方法

|                                 方法                                  |                           说明                           |
| :-------------------------------------------------------------------: | :------------------------------------------------------: |
| public Matcher appendReplacement(StringBuffer sb, String replacement) |                实现非终端添加和替换步骤。                |
|            public StringBuffer appendTail(StringBuffer sb)            |                 实现终端添加和替换步骤。                 |
|             public String replaceAll(String replacement)              |  替换模式与给定替换字符串相匹配的输入序列的每个子序列。  |
|            public String replaceFirst(String replacement)             |  替换模式与给定替换字符串匹配的输入序列的第一个子序列。  |
|            public static String quoteReplacement(String s)            | 返回指定字符串的字面替换字符串。这个方法返回一个字符串， |

### PatternSyntaxException 类的方法

- PatternSyntaxException 是一个非强制异常类，它指示一个正则表达式模式中的语法错误。
- PatternSyntaxException 类提供了下面的方法来帮助我们查看发生了什么错误。

|              方法              |                                              说明                                              |
| :----------------------------: | :--------------------------------------------------------------------------------------------: |
| public String getDescription() |                                        获取错误的描述。                                        |
|     public int getIndex()      |                                        获取错误的索引。                                        |
|   public String getPattern()   |                                   获取错误的正则表达式模式。                                   |
|   public String getMessage()   | 返回多行字符串，包含语法错误及其索引的描述、错误的正则表达式模式和模式中错误索引的可视化指示。 |
