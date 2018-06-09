---
title: Numpy-21-reshape
date: 2018-05-02 07:20:47
categories: Python
tags:
- Numpy
- shape
- reshape
---

# shape

修改ndarray的shape属性可以直接实现尺寸的转换，而且是当前ndarray直接起效。

```python
>>> import numpy as np
>>> a = np.arange(10)
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> a.shape = (2, 5)
>>> a
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
```



# reshape

```python
>>> import numpy as np
>>> a = np.arange(10)
>>> a.reshape([2, 5])
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

reshape只是复制原数列并作变形，返回另一个数列，所以需要接受此返回值。