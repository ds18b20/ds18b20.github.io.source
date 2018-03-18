---
title: Java-11-WrapperClasses
date: 2018-03-18 20:46:00
categories: Java
tags:
- Java
- Wrapper Classes
---

# 包装类

Java虽然是支持面向对象的编程，但是还是沿用了一些面向过程的习惯，比如Java的8种基本数据类型并不支持面向对象的编程。这8种基本数据类型没有对象的基本特征--属性和方法，比如一个int变量，不能查看它的属性，也不能调用它的方法。

这在OOP程序的编程中，由于不统一会带来一些不便。为了解决这个问题，Java引入了包装类的概念，即给每个基本数据类型设计了相应的类（包装类），也称为外覆类或者数据类型类。

**基本数据类型和包装类的对应关系**

| 基本数据类型 | 对应的包装类 |
| ------------ | ------------ |
| byte         | Byte         |
| short        | Short        |
| int          | Integer      |
| long         | Long         |
| char         | Character    |
| float        | Float        |
| double       | Double       |
| boolean      | Boolean      |

每个包装类的对象可以封装一个相应的基本类型的数据，并提供了其它一些有用的方法。包装类对象一经创建，其内容（所封装的基本类型数据值）不可改变。

基本类型和对应的包装类可以进行**相互转换**：

- 装箱：基本类型 > 包装类，如int基本类型转Integer类
- 拆箱：基本类型 > 包装类，如Integer类转int基本类型

例子，

```java
public class TestWrapperClass {
    public static void main(String[] args) {
        TestWrapperClass.int2integer();
        TestWrapperClass.str2int();
        TestWrapperClass.int2str();
    }
    // 手动装箱
    // 手动拆箱
    public static void int2integer() {
        int m = 500;
        Integer obj = new Integer(m); // 手动装箱
        int n = obj.intValue(); // 手动拆箱
        System.out.println("n = " + n);
        Integer obj1 = new Integer(500);
        System.out.println("obj equals obj1? " + obj.equals(obj1));
    }
    // string to int
    public static void str2int() {
        String str[] = { "123", "123abc", "abc123", "abcxyz" };
        for (String str1 : str) {
            try {
                int m = Integer.parseInt(str1, 10);
                System.out.println(str1 + " 可以转换为整数 " + m);
            } catch (Exception e) {
                System.out.println(str1 + " 无法转换为整数");
            }
        }
    }
    // int to string
    public static void int2str() {
        int m = 500;
        String s = Integer.toString(m);
        System.out.println("s = " + s);
    }
}
```

# 自动装箱和拆箱

Java1.5之后的版本支持自动装箱自动拆箱。这里的自动表现在，基本类型可以直接赋值给对应的包装类型，反之亦然。

```java
int m = 500;
Integer obj = m; // 自动装箱
int n = obj; // 自动拆箱
System.out.println("n = " + n);
```

虽然有自动功能，考虑到代码的可读性，推荐使用手动装箱拆箱。

# 参考

Java包装类、拆箱和装箱详解

https://www.cnblogs.com/ok932343846/p/6749488.html

Java装箱和拆箱--**这篇比较全面深入地讲解了常见的注意事项**

https://www.cnblogs.com/lsohvaen001/p/7823067.html