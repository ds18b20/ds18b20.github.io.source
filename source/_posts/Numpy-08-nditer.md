---
title: Numpy-08-nditer
date: 2017-11-29 00:16:23
categories: Python
tags:
- Numpy
- Python
- nditer
---

# numpy.ndarray

## single array iteration

```python
>>> import numpy as np
>>> a=np.arange(6).reshape(2,3)
>>> a
array([[0, 1, 2],
       [3, 4, 5]])

>>> for x in np.nditer(a):
...     print(x)
...
0
1
2
3
4
5
>>> for x in np.nditer(a.T):
...     print(x)
...
0
1
2
3
4
5
```

As **a** and **a.T** are stored in the same sequence, **np.nditer(a)** and **np.nditer(a.T)** have the same result.

## Control iteration sequence

**single index:** 单个索引

```python
# Fortran order
for x in np.nditer(a, order='F'):
    print(x)

# C order
for x in np.nditer(a.T, order='C'):
    print(x)
```

**multi index: **多重索引

```python
# multi_index
it = np.nditer(a, flags=['multi_index'])
while not it.finished:
    print('element: {}--id: {}'.format(it[0], it.multi_index))
    it.iternext()
```

既遍历了整个array也同时可以获取每个元素的下标multi_index，便于对元素进行读写操作。

注意，使用np.nditer进行循环遍历时**一定要记得加上it.iternext()语句**，否则会导致程序假死，无法向下执行。

## Modify element

```python
>>> for x in np.nditer(a, op_flags=['readwrite']):
...     x[...] = 2 * x
...
>>> a
array([[ 0,  2,  4],
       [ 6,  8, 10]])
```

By default, iteration use 'read-only'. To modify elements of arrays, op_flags should be assigned as 'readwrite'.

## external_loop

Use 'external_loop'

https://blog.csdn.net/lanchunhui/article/details/55657135