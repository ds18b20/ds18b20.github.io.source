---
title: Numpy-11-cumsum
date: 2018-02-05 23:41:38
categories: Python
tags:
- Python
- Numpy
- cumsum
---

# Numpy.cumsum

输入Array，返回由各位置处的累加和组成的新Array。

numpy.cumsum(a, axis=None, dtype=None, out=None)

a: 输入ndarray

axis: 指定计算所在的维度

# Examples

```python
>>> a = np.array([[1,2,3], [4,5,6]])
>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> np.cumsum(a)
array([ 1,  3,  6, 10, 15, 21])
>>> np.cumsum(a, dtype=float)     # specifies type of output value(s)
array([  1.,   3.,   6.,  10.,  15.,  21.])
```

```python
>>> np.cumsum(a,axis=0)      # sum over rows for each of the 3 columns
array([[1, 2, 3],
       [5, 7, 9]])
>>> np.cumsum(a,axis=1)      # sum over columns for each of the 2 rows
array([[ 1,  3,  6],
       [ 4,  9, 15]])
```

