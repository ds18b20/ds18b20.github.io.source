---
title: C-Warning-01-const
date: 2018-12-29 19:02:47
categories: C
tags:
- C
- const

---

# warning: discards 'const' qualifiers from pointer target type

首先这句警告的意思是，代码中把一个不可修改的指针类型const char * 转换成了char *，即移除了const修饰词。

这就使本来不能修改的const类型变量变成了可以修改的一般变量了，因此抛出了该警告。

https://stackoverflow.com/questions/37160873/warning-discards-const-qualifiers-from-pointer-target-type

https://blog.csdn.net/qq_36324796/article/details/79021258

另外，关于const修饰，可以参考https://blog.csdn.net/acorld/article/details/8921374