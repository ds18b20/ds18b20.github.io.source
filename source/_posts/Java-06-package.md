---
title: Java-06-Package
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

# reference
Java 语言基础
https://www.ibm.com/developerworks/cn/java/j-introtojava1/index.html

