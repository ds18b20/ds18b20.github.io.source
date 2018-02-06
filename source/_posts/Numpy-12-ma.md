---
title: Numpy-12-ma
date: 2018-02-06 22:39:08
categories: Python
tags:
- Numpy
- Python
- ma
---

# 掩码数组

Numpy的ma模块用于把ndarray的一部分mask掉，ndarray长度不变但被mask的地方变成制定的fill_value。

例如[1,2,3]用[0,1,0]来mask之后得到[1,--,3]

# 实例

```python
>>> import numpy as np

# 生成元数据的ndarray
>>> a=np.array([1,2,3,4,5,-1,8])
>>> a
array([ 1,  2,  3,  4,  5, -1,  8])

# 生成mask数组的ndarray
>>> mask = a>2
>>> mask
array([False, False,  True,  True,  True, False,  True],dtype=bool)

# 生成掩码数组
>>> mx=np.ma.array(a,mask=mask)
>>> mx
masked_array(data = [1 2 -- -- -- -1 --],
             mask = [False False  True  True  True False  True],
       fill_value = 999999)
# 掩码数组包含三个属性
#  data表示原始数据
#  mask表示生成掩码用的mask数组
#  fill_value表示替换值
>>> mx.data
array([ 1,  2,  3,  4,  5, -1,  8])
>>> mx.mask
array([False, False,  True,  True,  True, False,  True],dtype=bool)
>>> mx.fill_value
999999

# .filled()来显示替换后的掩码数组
>>> mx.filled()
array([     1,      2, 999999, 999999, 999999,     -1, 999999])

>>> mx[0] is np.ma.masked
False
>>> mx[2] is np.ma.masked
True

# 可以指定元素修改masked属性
>>> mx[0]=np.ma.masked
>>> mx.filled()
array([999999,      2, 999999, 999999, 999999,     -1, 999999])
>>>
```

同样适用于多维数组，mask数组也必须和元数据尺寸相同。

np.ma.asarray(x)把x转换成掩码数组，但不进行具体掩码操作。相当于初始化

# 参考

http://blog.csdn.net/baoqian1993/article/details/52116510

https://docs.scipy.org/doc/numpy/reference/maskedarray.baseclass.html