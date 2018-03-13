---
title: Java-04-Class
date: 2018-03-13 20:49:00
categories: Java
tags:
- Java
- Class
---

#Java中的类 

还是从最经典的例子开始，

```java
public class HelloWorld{
	public static void main(String[] args){
		System.out.println("Hello World");
	}
}
```

上述代码定义了一个名为HelloWorld的类。

针对java文件中只能有一个public类的问题，网上有人这么回答：

> 每个编译单元(文件)只能有一个public类。这么做的意思是，每个编 
>
> 译单元只能有一个公开的接口，而这个接口就由其public类来表示。
>
> 我想这或是从软件架构设计和安全性设计上得出的结论。或者说是java的设计者们从这方面的考虑。或许这真的是一个规范，但我没有找到相关资料
>
> 不晓得到底有没有这一说话。如果有请知道的同行给出资料来源？

<http://topic.csdn.net/t/20060528/22/4784755.html>

## .java包含public类时

另外，需要注意的是，包含public类的java源文件必须以public类的名字来命名。

上面的HelloWorld的例子中，如果把java文件名换成其他，则编译器会报错。

`名为xxx的公开类必须在名为xxx.java的文件中声明。`

而且，一个java文件中最多只能包含一个public类。

作为Java的一种设计思想，使public类名和java文件名统一，便于从其他类中调用。

## .java不包含public类时

java源文件中可以不包含public类，这时java文件名可以不和其中的任一类名一致。

```java
/*C3.java*/
// 
class C1{
	int i = 1;
}
// 
class C2{
	public static void main(String[] args){
		System.out.println("Hello World.");
	}
}
```

Command line:

```
>>> javac C3.java
>>> java C2
```

这时，正常输出Hello World.

反而是运行java C3会报错，因为没有定义C3类，提示找不到该类。

从上亦可以，main函数不一定非得包含在public类中也可以运行。

《深入jvm第二版》中有这样一句话：

> Java虚拟机实例通过调用某个类的main()来运行一个Java程序，而这个main()必须是public static void 并接收一个字符串数组作为参数，任何拥有这样一个main()的类都可以作为Java程序的起点。

并没有说拥有main()方法的类一定要是public类。

参考：

浅谈为什么一个java源文件中只能有一个public类？

http://blog.csdn.net/bareheadzzq/article/details/6562211

# main函数


main函数中，
public定义了函数的访问权限，表示在任何场合都可以被引用，这样JVM就可以找到main方法从而运行javac程序。

static表示此方法是静态的，是类的方法。可以使用`类名.方法名`的方式调用。
不需要初始化得到实例，再调用实例的方法。

void和C语言中的规定相同，表示函数没有返回值。

String[] args表示参数args是一个字符串数组，用于给main函数传递参数。
例如，命令行下运行java ClassName xyz abc
在main函数体内就可以使用args字符串数组了。
args[0]表示第一个元素，即xyz

String[] args和String args[]运行结果一样，但是应使用前者来避免阅读代码的人的歧义和误读（此处还需深入理解）

参考：

在java中public void与public static void有什么区别 ?
https://www.zhihu.com/question/39965419

## print/println

此外Java还提供了print函数，和println函数的区别是，println默认在输出的最后加上了换行。

无论是print还是println，参数内都可以传入运算表达式或者变量。如，

```java
System.out.print(1+2);

String str = "abc";
System.out.println(str);
```

参考：
http://www.runoob.com/java/java-tutorial.html

