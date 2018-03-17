---
title: Java-06-package-Class
date: 2018-03-17 12:26:35
categories: Java
tags:
- Java
- Package
- Class
---

# 类的成员
## 成员的访问权限
- private
- (default)
- protected
- public

比较难记，参考 https://www.cnblogs.com/jingmengxintang/p/5898900.html
另外需要验证实例是否可以访问类的private成员。

类的成员包括属性和方法

## 属性

属性定义了类的状态，定义方式如下，

`accessSpecifier dataType variableName [= initialValue];`

accessSpecifier：访问权限

dataType：类型

variableName：属性名

initialValue：初始化值

## 方法

方法定义了类的行为，分为**构造方法**和**其他方法**。

**构造方法**仅用于创建对象的实例，它的书写有三点要求：

1. 构造方法的accessSpecifier必须和类的相同
2. 构造方法名必须和类名相同
3. 不能含有返回类型，void也不可

构造方法内，要对实例的属性进行赋值时需要`this.attribute`

```java
package com.makotojava.intro;
public class Person {
  private String name;
  private int age;
  private int height;
  private int  weight;
  private String eyeColor;
  private String gender;

  public Person(String name, int age, int height, int weight String eyeColor, String gender) {
    this.name = name;
    this.age = age;
    this.height = height;
    this.weight = weight;
    this.eyeColor = eyeColor;
    this.gender = gender;
  }
}
```



其他方法的命名不要求和class一致，而且可以有自己的返回值。

```java
	...
	public String getName(){
        return name;
	}
    public void setName(String value){
        name = value;
	}
	...
```

注意如果一个方法没有返回值，也必须加上void关键字，以告知编译器。

非构造方法又可以分为两类：**静态方法**和**实例方法**。

静态方法直接通过类调用；而实例方法必须先生成一个实例，然后由实例调用。

#  Eclipse操作

## 创建package

单击 **File > New > Package** 启动 Java Package 向导，将 `com.makotojava.intro` 键入到 Name 文本框中并单击**Finish**。可以在 Package Explorer 中看到创建的新包。



## 声明class

右键上述新建的package，选择 **New > Class...**。New Class 对话框将会打开。

在 Name 文本框中，键入 `Person`，然后单击 **Finish**。

## 添加class的属性

得到，

```java
package com.makotojava.intro;
 
public class Person {
  private String name;
  private int age;
  private int height;
  private int weight;
  private String eyeColor;
  private String gender;
}
```

## 添加class的方法

包括构造方法和其他方法。

其他方法中的getter和setter可以由Eclipse快捷添加，右键 > source > generate getter and setter 设置相应选项即可。

得到，



    package com.makotojava.intro;
    
    public class Person {
        private String name;
        private int age;
        private int height;
        private int weight;
        private String eyeColor;
        private String gender;
    
        public Person(String name, int age, int height, int weight, String eyeColor, String gender) {
            this.name = name;
            this.age = age;
            this.height = height;
            this.weight = weight;
            this.eyeColor = eyeColor;
            this.gender = gender;
        }
    
        public static void main(String[] args) {
            // TODO Auto-generated method stub
    
        }
    
        public String getName() {
            return name;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    	...
    }

}

# reference

包含构造方法的类定义
https://www.ibm.com/developerworks/cn/java/j-introtojava1/index.html

java构造方法与方法的区别
http://blog.csdn.net/lvshihua/article/details/38409121

Java 语言基础

https://www.ibm.com/developerworks/cn/java/j-introtojava1/index.html