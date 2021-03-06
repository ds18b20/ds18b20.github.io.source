---
title: Numpy-00-Errors
date: 2018-03-29 22:35:32
categories: Python
tags:
- Python
- Numpy
- Errors

---

# dtype的选择

```python
# type
import numpy as np

x = np.array([0, 0], dtype=np.int)
print("x: {}".format(x))
x[0] = 3.14
print("x: {}".format(x))

y = np.array([0, 0], dtype=np.float)
print("y: {}".format(y))
y[0] = 3.14
print("y: {}".format(y))
```

输出

```python
x: [0 0]
x: [3 0]
y: [ 0.  0.]
y: [ 3.14  0.  ]
```

可以看到，虽然修改x[0]为一个浮点数3.14，但是由于x的类型是np.int，x[0]只保留了3.14的整数部分。

而y的类型是np.float，修改y[0]为3.14后，小数也能完整保留。

# Unresolved attribute reference 'mean' for class 'bool'

```
(y_hat.argmax(axis=1) == y).mean()
# or
np.mean(y_hat.argmax(axis=1) == y)
```

上述代码会提示bool对象不包含mean方法。

> Unresolved attribute reference 'mean' for class 'bool'

原因是PyCharm不能很好的识别numpy.ndarray对象，也就把上述==计算的结果存储为bool对象，进而提示bool不包含mean方法。
解决方法，可以在表达式后面加上# type: np.ndarray来提示IDE表达式结果的类型。

```
tmp = y_hat.argmax(axis=1) == y  # type: np.ndarray
np.mean(tmp)
```

# 参考

Expected type 'Union[ndarray, Iterable]' warning in Python instruction
https://stackoverflow.com/questions/30599676/expected-type-unionndarray-iterable-warning-in-python-instruction