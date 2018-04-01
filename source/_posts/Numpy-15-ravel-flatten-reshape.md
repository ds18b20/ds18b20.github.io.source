---
title: Numpy-15-ravel-flatten-reshape
date: 2018-03-31 16:27:55
categories: Python
tags:
- Numpy
- Python
- flatten
- ravel
- reshape
---

# 功能

从结果上看，三者都可以实现把一个多维的array压平为一维。ravel和flatten的区别在于是返回拷贝(copy)还是返回视图(view)，对copy做的修改不会影响原array。

添加内容中。。。

# ravel



# flatten



# reshape(-1)

`array.reshape(x, -1)`只关注行数x，列数自动计算。

# 参考

numpy 辨异 （五）—— numpy.ravel() vs numpy.flatten()

https://blog.csdn.net/lanchunhui/article/details/50354978