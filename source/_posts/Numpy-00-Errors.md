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

