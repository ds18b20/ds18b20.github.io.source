---
title: Python-12-generator
date: 2018-02-07 23:46:18
categories: Python
tags:
- Python
- Generator
---

# 生成器

通过列表生成(list comprehension)可以轻松创建一个新的list。

但是当此list很大时，直接生成的方式未必是个好的选择，因为这会占用大量的内存空间，浪费计算能力。Python中可以通过一边计算一边生成的方式来得到在循环中使用的元素。这就是生成器(generator)

# 创建方法1

创建generator的方式之一，直接把列表生成器的[]换成()即可

```python
>>> L = [x*x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

>>> G = (x*x for x in range(10))
>>> G
<generator object <genexpr> at 0x01F91BA0>
```

要打印generator的各元素可以一直next(generator)或者generator.next()下去，直到计算到最后一个元素，然后抛出错误。

```python
>>> next(G)
0
>>> next(G)
1
>>> next(G)
4
>>> next(G)
9
>>> next(G)
16
>>> next(G)
25
>>> next(G)
36
>>> next(G)
49
>>> next(G)
64
>>> next(G)
81
>>> next(G)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

由于generator对象也是可迭代对象，故可以使用for循环来输出

```python
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
```

# 创建方法2-生成器函数

当推算算法比较复杂时，无法直接通过类似列表生成式的方式实现时，可以通过函数来实现。

在函数中加入yield关键字来实现。参见

https://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/