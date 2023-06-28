---
title: Java反射
date: 2021-12-15
tags: Java
author: 映雪
---

咫尺天涯处，似假几若真。

<!--more-->

### Java Reflection

1. Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期 借助于ReflectionAPI取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

2. 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以称之为：反射。


### 区别

![截屏2023-04-03 上午11.34.24.png](/images/2023/04/03/9XT67yK1EgMurtW.png)

### 反射的作用

1. 得到一个对象所属的类，
2. 获取一个类的所有成员变量和方法，
3. 在运行时创建对象，调用对象的方法

### 反射机制的实现

1. 通过类名获取 类名.calss
2. 通过对象获取 对象.getClass
3. 通过全类名获取 Class.forName(“类路径.类名”)

```java
public class Fanshe {
	public static void main(String[] args) {
		//第一种方式获取Class对象  
		Student stu1 = new Student();//这一new 产生一个Student对象，一个Class对象。
		Class stuClass = stu1.getClass();//获取Class对象
		System.out.println(stuClass.getName());
		
		//第二种方式获取Class对象
		Class stuClass2 = Student.class;
		System.out.println(stuClass == stuClass2);//判断第一种方式获取的Class对象和第二种方式获取的是否是同一个
		
		//第三种方式获取Class对象
		try {
			Class stuClass3 = Class.forName("fanshe.Student");//注意此字符串必须是真实路径，就是带包名的类路径，包名.类名
			System.out.println(stuClass3 == stuClass2);//判断三种方式是否获取的是同一个Class对象
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
	}
}

```

### 反射的具体应用

1. 原来使用new的时候，需要明确的指定类名，这个时候属于硬编码实现，而在使用反射的时候，可以只传入类名参数，就可以生成对象，降低了耦合性，使得程序更具灵活性。关于这一点的实例有：简单工厂模式的优化、Spring框架中Bean的创建、动态代理的实现等。
2. 原来并未使用反射时，我们没办法在运行时动态获取/修改一个类的所有属性，而通过反射机制，我们能够在运行时确定类的状态和属性，这为灵活操作提供了空间。具体的实例有：运行时根据类的状态进行异常监测，突破封装限制获取修改private、protected属性，这点在IDE的调试器中就有应用。

### 反射机制常用的类：

```
Java.lang.Class;
Java.lang.reflect.Constructor;
Java.lang.reflect.Field;
Java.lang.reflect.Method;
Java.lang.reflect.Modifier;
```

### 反射越过泛型检查

-  泛型是在编译期间起作用的。在编译后的.class文件中是没有泛型的。所有比如T或者E类型，本质都是通过Object处理的。所以可以通过使用反射来越过泛型。

```java
package test;
public class GenericityTest {
	public static void main(String[] args) throws Exception{
		
	ArrayList<String> list = new ArrayList<>();
	list.add("this");
	list.add("is");
	
	//	strList.add(5); 会报错
	
	//获取ArrayList的Class对象，反向的调用add()方法，添加数据
	Class listClass = list.getClass(); 
	//获取add()方法
	Method m = listClass.getMethod("add", Object.class);
	//调用add()方法
	m.invoke(list, 5);
	
	//遍历集合
	for(Object obj : list){
		System.out.println(obj);
		}
	}

}


```