---
title: Java-06-package
date: 2018-03-15 23:43:35
categories: Java
tags:
- Java
- Package
---

# 打包(package)类
Java程序是由每个类互相调用实现功能的，当类数量较多时，怎样合理管理这些类是一个问题。

Java中类拥有自己专属的名称，为了保持调用的准确性，每个命名空间中类的命名必须具有唯一性。
但是当项目较大时，很难保证class的名字不重复。因此引入package的概念来定义命名空间，避免导入时带来的冲突。

class在package提供的命名空间内不可重复，但是在其外面可以重复。

## 定义包
定义包使用package关键字，其后跟着包的名称，最后需要分号来结尾。包
的名称约定用分层的方式定义，即，
package orgType.orgName.appName.compoName;

这个包定义可分解为不同的部分：

- orgType 是组织类型，比如 com、org 或 net。
- orgName 是组织的域名称，比如 makotojava、oracle 或 ibm。
- appName 是应用程序的缩写名称。
- compoName 是组件的名称。


## 导入包
导入包使用import关键字，后面跟着要导入的包名，也是以分号结尾。对应于包的定义方式，包的导入也是分层实现，
直到能精确定位到类。

import com.makotojava.Abc;

导入所有的类时，可以`import com.makotojava.*;`
但是并不推荐这样书写，会降低代码的可读性。

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

# reference
包含构造方法的类定义
https://www.ibm.com/developerworks/cn/java/j-introtojava1/index.html

java构造方法与方法的区别
http://blog.csdn.net/lvshihua/article/details/38409121

