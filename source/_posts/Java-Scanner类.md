---
title: Scanner类
date: 2020-12-26
tags: Java
author: 映雪
---

大雾四起 ，我在无人之处爱你 。大雾消散 ，我爱你人尽皆知。

<!--more-->

### Scanner类

- java.util.Scanner是Java5的新特征，可以通过 Scanner 类来获取用户的输入。

```java
Scanner s = new Scanner(System.in); 
```

### Scanner类方法

1. **next()**

- 一定要读取到有效字符后才可以结束输入。
- 对输入有效字符之前遇到的空白，next()方法会自动将其去掉。
- 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。

```java
import java.util.Scanner; 

public class ScannerDemo {  
    public static void main(String[] args) {  
        Scanner scan = new Scanner(System.in); 
		// 从键盘接收数据  

		//next方式接收字符串
        System.out.println("next方式接收：");
        // 判断是否还有输入
        if(scan.hasNext()){   
        	String str1 = scan.next();
        	System.out.println("输入的数据为："+str1);  
        }  

    }  
} 
```

2. **nextLine()**

- 以Enter为结束符,也就是说nextLine()方法返回的是输入回车之前的所有字符。
- 可以获得空白。

```java
mport java.util.Scanner; 

public class ScannerDemo {  
    public static void main(String[] args) {  
        Scanner scan = new Scanner(System.in); 
		// 从键盘接收数据  

		//nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if(scan.hasNextLine()){   
        	String str2 = scan.nextLine();
        	System.out.println("输入的数据为："+str2);  
        }  
    }  
} 
```

3. **nextXXX()**

- 输入int或float类型的数据

```java
import java.util.Scanner;  

public class ScannerDemo {  
    public static void main(String[] args) {  
        Scanner scan = new Scanner(System.in);  
		// 从键盘接收数据  
        int i = 0 ;  
        float f = 0.0f ;  
        System.out.print("输入整数：");  
        if(scan.hasNextInt()){                 
			// 判断输入的是否是整数  
            i = scan.nextInt() ;                
			// 接收整数  
            System.out.println("整数数据：" + i) ;  
        }else{                                 
			// 输入错误的信息  
            System.out.println("输入的不是整数！") ;  
        }  
        System.out.print("输入小数：");  
        if(scan.hasNextFloat()){              
			// 判断输入的是否是小数  
            f = scan.nextFloat() ;             
			// 接收小数  
            System.out.println("小数数据：" + f) ;  
        }else{                                
			// 输入错误的信息  
            System.out.println("输入的不是小数！") ;  
        }  
    }  
} 
```

