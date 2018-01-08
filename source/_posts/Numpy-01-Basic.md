---
title: Numpy-01-Basic
date: 2017-11-08 23:43:02
categories: Python
tags:
- Python
- Numpy
- Basic
---

# Basic of Numpy

## Create array

```python
import numpy as np

X = np.array([[1,2],[2,3],[3,4]])
```

## Slicing and Indexing

```python
>>> X.flatten()
array([1, 2, 2, 3, 3, 4])

>>> X[range(2)]
array([[1, 2],
       [2, 3]])

>>> X[np.array([0,2])]
array([[1, 2],
       [3, 4]])

>>> X[0]
array([1, 2])

>>> X[0][1]
2

>>> for row in X:
...     print(row)
...
[1 2]
[2 3]
[3 4]
```

## Change dimensions

Use  to  numpy.ndarray.reshape method to change dimensions.

```python
L = [1, 2, 3]
A = np.array(L)
B = np.array(L).reshape(3, 1)
```

## Compare 2 ndarrays

```python
>>> x
array([1, 2, 3, 1, 5])
>>> y
array([1, 3, 3, 4, 5])

# compare
>>> x==y
array([ True, False,  True, False,  True], dtype=bool)
>>> (x==y).astype(np.int)
array([1, 0, 1, 0, 1])

# or use np.sum directly
>>> np.sum(x==y)
3
```

