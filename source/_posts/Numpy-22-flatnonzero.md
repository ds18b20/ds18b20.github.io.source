---
title: Numpy-22-flatnonzero
date: 2018-05-09 19:48:57
categories: Python
tags:
- Numpy
- flatnonzero
---

#np.flatnonzero

`numpy.flatnonzero`(*a*)

Return indices that are non-zero in the flattened version of a.

This is equivalent to a.ravel().nonzero()[0].

| Parameters: | **a** : ndarrayInput array.                                  |
| ----------- | ------------------------------------------------------------ |
| Returns:    | **res** : ndarrayOutput array, containing the indices of the elements of *a.ravel()* that are non-zero. |

## example

```python
>>> import numpy as np

>>> x = np.arange(-2, 3)
>>> x
array([-2, -1,  0,  1,  2])
>>> np.flatnonzero(x)
array([0, 1, 3, 4], dtype=int64)
>>> s = np.flatnonzero(x)
>>> x[s]
array([-2, -1,  1,  2])
```

np.flatnonzero(x)静态方法返回序列x中所有非0元素的下标位置，上例[-2, -1,  0,  1,  2]中非零元素的位置为[0, 1, 3, 4]，故返回序列[0, 1, 3, 4]。返回的序列可以继续用来对其他/本序列做slice。