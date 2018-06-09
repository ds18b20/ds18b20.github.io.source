---
title: Math-02-SoftMax
date: 2018-04-07 14:23:08
categories: Math
tags:
- Math
- Softmax
---

# softmax函数

函数的定义如下，
$$
\begin{eqnarray} 
  a^L_j = \frac{e^{z^L_j}}{\sum_k e^{z^L_k}},
\tag{1}\end{eqnarray}
$$
softmax函数的输出有以下特点，

1. 输出值介于(0, 1]之间；
2. 输入序列中最大项占序列总和的比例，经过softmax函数后，会大幅度扩大。简言之，大的越大，小的越小。

代码显示如下，

```python
from common.functions import softmax
import numpy as np
import matplotlib.pyplot as plt

x = np.array([10,11,9])

a = x / np.sum(a)
print("原始比例: {}".format(a))
b = softmax(x)
print("Softmax比例: {}".format(b))

# Two subplots, the axes array is 1-d
f, axarr = plt.subplots(2, sharex=True)
axarr[0].bar(np.arange(3),a)
axarr[0].set_title('Sharing X axis')
axarr[1].bar(np.arange(3),b, color='r')
```

输出结果为，

![](Math-02-SoftMax\softmax.png)

# 注意防止溢出

$$
y_j=\frac{e^{a_{k}}}{\sum^{N}_{i=1}e^{a_{i}}}
$$

应用softmax函数时，应该注意防止分子分母过大带来的计算溢出。

由于包含指数运算，当a_i绝对值过大时，e^a_i会指数级增大，这就会带来overflow溢出错误。可以注意到其实各元素的a_i的差值并不是很大，处理方法为，分子分母同时除以一个e^C，其中C = Max(a_i)。
$$
y_{j}=\frac{e^{a_{k}}-C}{\sum^{N}_{i=1}e^{a_{i}}-C}
$$

$$
C=max({a_{i}})
$$

就可以大幅减小计算数值，这样就基本不会发生overflow了。

事实上，差值过大时，也不用SoftMax函数来增强输出效果了。