---
title: Numpy-07-sum
date: 2017-11-27 22:40:25
categories: Python
tags:
- Numpy
- Python
- random
---

# Numpy.sum(ndarray, axis=1)

## sum

```python
import numpy as np

>>> a=np.arange(10).reshape(2,5)
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
>>> np.sum(a)
45
>>> np.sum(a,axis=1)
array([10, 35])
>>> np.sum(a,axis=0)
array([ 5,  7,  9, 11, 13])
```



![NumpySum.png](Numpy-07-sum/NumpySum.png)

[Image from](https://www.cnblogs.com/yyxayz/p/4033736.html)

## average

类似地，

```python
>>> np.average(a,axis=1)
array([2., 7.])
>>> np.average(a,axis=0)
array([2.5, 3.5, 4.5, 5.5, 6.5])
```

