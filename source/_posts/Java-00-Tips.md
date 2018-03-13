---
title: Java-00-3-Tips
date: 2018-03-11 12:39:08
categories: Java
tags:
- Java
- Tips
---

# 运行.class bytecode

命令行下使用`java .class`运行.class字节码。

注意：不可以带后缀名。



# Java命名规范

fileName

folderName

niceToMeetYou

统一使用驼峰式书写规范。



# JLabel背景颜色

给JLabel对象添加背景颜色时，需要

```java
// set up textJLabel
textJLabel = new JLabel();
textJLabel.setBackground(Color.MAGENTA);
```

这样并不能正常显示bg颜色，因为还没设置opaque(不透明显示)，且默认为transparent(透明显示)。

故需要添加一行，

```java
// set up textJLabel
textJLabel = new JLabel();
// opaque
textJLabel.setOpaque(true);
textJLabel.setBackground(Color.MAGENTA);
```

