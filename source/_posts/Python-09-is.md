---
title: Python-09-is
date: 2018-02-06 22:46:54
categories: Python
tags:
- Python
- is
---

# 基础准备

Python对象包括三个基本要素：id，type和value。其中id用来唯一标示一个对象；type用于标示对象的类型，`这里注意一下type()和isinstance()的区别`；而value是对象的值。

`还有，可以参考一下可变量和不可变量。`

# ==和is的区别

简单地讲，`a is b`用来判断a和b是不是指向同一个对象，依据id()来判断。

而`a ==b`是判断a和b的value是否相等。比如1 == 1.0，但是id(1) <>id(1.0)

另外，由于数组是可变对象，故两个包含相同数据的数组==，但不是is