---
title: Numpy-18-astype
date: 2018-04-22 10:11:49
categories: Python
tags:
- Numpy
- astype
---

# Numpy.astype

Numpy的的整数类型默认为'int32'

```python
>>> import numpy as np
>>>
>>> a = np.arange(12).reshape(3,4)
>>> a.dtype
dtype('int32')
```

默认为`dtype('int32')`

要更改ndArray的data类型时，使用.astype()方法。

```python
>>> b = a>5
>>> b
array([[False, False, False, False],
       [False, False,  True,  True],
       [ True,  True,  True,  True]])

>>> c = b.astype(np.int)
>>> c
array([[0, 0, 0, 0],
       [0, 0, 1, 1],
       [1, 1, 1, 1]])
```

b输出为，

```
[[False False False False]
 [False False  True  True]
 [ True  True  True  True]]
```

b经过.astype()函数之后，得到c

```
[[0 0 0 0]
 [0 0 1 1]
 [1 1 1 1]]
```

