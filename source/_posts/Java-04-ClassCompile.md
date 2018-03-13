---
title: Java-04-Class
date: 2018-03-13 20:52:00
categories: Java
tags:
- Java
- Class
- Compile
---

#Java中类的编译 

## 多个类并列存在

```java
// 
class A{
	int i = 1;
}
// 
class B{
	int i = 2;
}
// 
class C{
	public static void main(String[] args){
		System.out.println("Hello World.");
	}
}
```

Command line:

```
>>> javac C3.java
```

编译得到A.class B.class 和 C.class三个字节码文件。

## 内部类 nested class

```java
// 
class A{
	int i = 1;
	// 
    class B{
    	int i = 2;
    }
}
```

编译后会生成两个.class文件：A.class和A$B.class

## 匿名类

编译后也是生成两个.class文件：A.class和A$1.class

(还没学习到)

# 参考

一个.java源文件中可以有多个类吗？（内部类除外）有什么条件？

http://blog.csdn.net/wikijava/article/details/4064688