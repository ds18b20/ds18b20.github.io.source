---
title: Numpy-23-Numpy2NativeType
date: 2018-05-16 21:19:45
tags:
- Numpy
- Native type
---

# Convert Numpy to Python native type

```python
>>> import numpy as np
>>> a=np.array([1,2,3])
>>> b=np.sum(a)
```

使用item方法可以把Numpy类型转换成Python原生类型。

但是转换只能是以单个元素为单位，不能整体转换。

```python
>>> type(b.item())
<class 'int'>
>>> b.item()
6

>>> a.item()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: can only convert an array of size 1 to a Python scalar
    
>>> for i in a:
...    print(type(i),type(i.item()))
...
<class 'numpy.int32'> <class 'int'>
<class 'numpy.int32'> <class 'int'>
<class 'numpy.int32'> <class 'int'>
```

# 参考

[Converting numpy dtypes to native python types](https://stackoverflow.com/questions/9452775/converting-numpy-dtypes-to-native-python-types)