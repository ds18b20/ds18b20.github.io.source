---
title: Numpy-06-random
date: 2017-11-15 21:36:32
categories: Python
tags:
- Numpy
- Python
- random
---

# numpy.random

## numpy.random.rand()

numpy.random.rand(d0,d1,…,dn)

Use .rand to get one or a list of random sample from [0,1).

```python
>>> np.random.rand()
0.18764147324357994
>>> np.random.rand(2,3)
array([[ 0.50590041,  0.37166178,  0.86995256],
       [ 0.16217236,  0.88082289,  0.98760503]])
```

## numpy.random.randn()

numpy.random.randn(d0,d1,…,dn)

Use .randn to get one or a list of standard normal distribution sample.

```python
>>> np.random.randn(2,3)
array([[-0.08735649, -1.20363118, -0.67110949],
       [ 0.00484652,  0.22180857,  0.25103274]])
```

## numpy.random.randint()

numpy.random.randint(low, high=None, size=None, dtype=’l’)

```python
>>> np.random.randint(0,10,size=(2,3))
array([[1, 2, 5],
       [8, 8, 9]])
```

## numpy.random.choice()

numpy.random.choice(a, size=None, replace=True, p=None)

`a : 1-D array-like or int    If an ndarray, a random sample is generated from its elements.     If an int, the random sample is generated as if a was np.arange(n)size : int or tuple of ints, optional replace : boolean, optional    Whether the sample is with or without replacementp : 1-D array-like, optional    The probabilities associated with each entry in a. If not given the sample assumes a uniform distribution over all entries in a.`

```python
>>> np.random.choice(10)
2
>>> np.random.choice(10,10)
array([3, 2, 0, 6, 0, 0, 2, 2, 4, 5])
>>> np.random.choice(10,(2,3))
array([[2, 2, 3],
       [0, 4, 1]])
```

