---
title: Numpy-02-MaxandMaximum
date: 2017-11-10 23:38:15
categories: Python
tags:
- Numpy
- Python
- Max
- Maximum
---

# Max and Maximum

## numpy.max

`np.max：(a, axis=None, out=None, keepdims=False) `

To get max from an array:

```python
>>> a=np.arange(6).reshape(3,2)
>>> a
array([[0, 1],
       [2, 3],
       [4, 5]])
>>> np.max(a)
5
```

## numpy.maximum

`np.maximum：(X, Y, out=None) `

To get 'max-array' by comparing each element from two arrays:

```python
>>> a=np.array([[1,0],[2,1],[5,3]])
>>> b=np.array([[1,1],[1,1],[1,1]])
>>> np.maximum(a,b)
array([[1, 1],
       [2, 1],
       [5, 3]])
>>> np.maximum(a,1) # Numpy broadcast
array([[1, 1],
       [2, 1],
       [5, 3]])
```

