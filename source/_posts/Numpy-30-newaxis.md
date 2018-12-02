---
title: Numpy-30-newaxis
date: 2018-11-17 17:34:22
categories: Python
tags:
- Numpy
- newaxis

---

# numpy.newaxis

用numpy.newaxis可以给ndarray对象增加一个维度。

以下是官方示例的引用：

```python
>>> import numpy as np
>>> np.newaxis is None
True
>>> x = np.arange(3)
>>> x
array([0, 1, 2])
>>> x[:, np.newaxis]
array([[0],
       [1],
       [2]])
```

另外，如果要压缩某一空白维度，可以参考numpy.squeeze方法。

```
numpy.squeeze(a, axis=None)[source]
Remove single-dimensional entries from the shape of an array.
```

# Reference

https://docs.scipy.org/doc/numpy-1.15.1/reference/constants.html#numpy.newaxis