---
title: Numpy-10-ArrayMerge
date: 2017-12-09 14:13:59
categories: Python
tags:
- Numpy
- Python
- vstack
- row_stack
- hstack
- column_stack
- concatenate
---

# Merge Numpy Array

## Merge by row

Stack arrays in sequence vertically (row wise).

`numpy.vstack()` can be replaced by `numpy.row_stack()`

CODE:

```python
>>> import numpy as np

>>> a=np.array([[1, 2, 3],[4, 5, 6]])
>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> b=np.array([[10, 20, 30],[40, 50, 60]])
>>> b
array([[10, 20, 30],
       [40, 50, 60]])

# vstack
>>> np.vstack((a,b))
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [10, 20, 30],
       [40, 50, 60]])
# row_stack
>>> np.row_stack((a,b))
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [10, 20, 30],
       [40, 50, 60]])
```

## Merge by column

Stack arrays in sequence horizontally (column wise).

`numpy.hstack()` can be replaced by `numpy.row_stack()`

```python
# hstack
>>> np.hstack((a,b))
array([[ 1,  2,  3, 10, 20, 30],
       [ 4,  5,  6, 40, 50, 60]])
# column_stack
>>> np.column_stack((a,b))
array([[ 1,  2,  3, 10, 20, 30],
       [ 4,  5,  6, 40, 50, 60]])
```

## numpy.concatenate

```python
# axis=0
>>> np.concatenate((a,b),axis=0)
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [10, 20, 30],
       [40, 50, 60]])
# axis=1
>>> np.concatenate((a,b),axis=1)
array([[ 1,  2,  3, 10, 20, 30],
       [ 4,  5,  6, 40, 50, 60]])
```

