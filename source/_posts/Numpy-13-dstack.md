---
title: Numpy-13-dstack
date: 2018-03-31 14:52:44
categories: Python
tags:
- Numpy
- zip
- dstack
---

# zip 1d arrays

```python
import numpy as np

a = np.array([0,1,2])
b = np.array([3,4,5])
# use zip
x = list(zip(a,b))
print(x)
# use np.dstack
y = np.dstack((a,b))
print(y)
```

得到，

```
[(0, 3), (1, 4), (2, 5)]
[[[0 3]
  [1 4]
  [2 5]]]
```

这里np.stack和内建的zip函数可以实现相同的功能。只是，输出的内部数据结构有区别，zip得到的是tuple组成的list，np.stack得到的是一个2维array。

# zip 2d arrays

## 使用zip

```
import numpy as np

x,y = np.mgrid[0:5, 0:3]
# use zip
xy = [list(zip(x, y)) for x, y in zip(x, y)]

# change list 2 array
xy_array = np.array(xy)
```

得到list类型的xy：

```
[[(0, 0), (0, 1), (0, 2)], [(1, 0), (1, 1), (1, 2)], [(2, 0), (2, 1), (2, 2)], [(3, 0), (3, 1), (3, 2)], [(4, 0), (4, 1), (4, 2)]]
```

## 使用np.stack

使用np.dstack时，

```
import numpy as np

x, y = np.mgrid[0:5, 0:3]
print(np.dstack((x, y)))
```

得到，

```
[[[0 0]
  [0 1]
  [0 2]]

 [[1 0]
  [1 1]
  [1 2]]

 [[2 0]
  [2 1]
  [2 2]]

 [[3 0]
  [3 1]
  [3 2]]

 [[4 0]
  [4 1]
  [4 2]]]
```

# 参考

in Numpy, how to zip two 2-D arrays?

https://stackoverflow.com/questions/17960441/in-numpy-how-to-zip-two-2-d-arrays