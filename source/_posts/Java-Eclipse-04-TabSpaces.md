---
title: Java-Eclipse-03-CheckStyle
date: 2018-03-13 22:13:00
categories: Java
tags:
- Java
- eclipse
- Tab
- Spaces
---

新版Eclipse默认Tab不转换为Spaces，为了各编辑器统一起见，推荐替换Tab为4个spaces。

# 设置方法

需要修改两个部分，公共设置和Java语言部分

## 公共部分

Preference-->General->Editors-->Text Editors，勾上Insert spaces for tabs。

## Java语言设置

Java-->Code Style-->Formatter-->New 新建一份文件，将Indentation 中的 Tab policy改为Spaces only。

以后如果添加了其他语言，同样需要在各语言下修改此设置。

## reference

使用eclipse中Tab格式兼容问题

http://blog.csdn.net/backend_research/article/details/53332454



# 其他设置

包括自动添加类的注释之类的方便功能。。。

http://blog.csdn.net/hotdust/article/details/52205867

