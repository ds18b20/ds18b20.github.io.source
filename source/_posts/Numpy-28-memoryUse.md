---
title: Numpy-28-memoryUse
date: 2018-08-23 23:32:22
categories: Python
tags:
- Numpy
- memory use

---

# Numpy.ndarray memory use

使用Numpy进行科学计算时，经常需要预估数据所占的内存容量，从而避免Memory错误。

- 要查看各对象的内存占用可以使用sys模块的getsizeof方法。
- 要查看Numpy对象内部的元素所占的空间可以使用numpy.ndarray.nbytes来查看所有数组占用的空间大小；使用numpy.ndarray.itemsize来查看单个元素占用的空间大小。

## sys.getsizeof

```python
import numpy as np
import sys

# 32位整型
ai32 = np.array([], dtype=np.int32)
bi32 = np.arange(1, dtype=np.int32)
ci32 = np.arange(5, dtype=np.int32)

# 64位整型
ai64 = np.array([], dtype=np.int64)
bi64 = np.arange(1, dtype=np.int64)
ci64 = np.arange(5, dtype=np.int64)

# 32位浮点数
af32 = np.array([], dtype=np.float32)
bf32 = np.arange(1, dtype=np.float32)
cf32 = np.arange(5, dtype=np.float32)

# 64位浮点数
af64 = np.array([], dtype=np.float64)
bf64 = np.arange(1, dtype=np.float64)
cf64 = np.arange(5, dtype=np.float64)

print("size of 0 int32 number: %s" % sys.getsizeof(ai32))
print("size of 1 int32 number: %s" % sys.getsizeof(bi32))
print("size of 5 int32 numbers: %s" % sys.getsizeof(ci32), end='\n\n')

print("size of 0 int64 number: %s" % sys.getsizeof(ai64))
print("size of 1 int64 number: %s" % sys.getsizeof(bi64))
print("size of 5 int64 numbers: %s" % sys.getsizeof(ci64), end='\n\n')

print("size of 0 float32 number: %s" % sys.getsizeof(af32))
print("size of 1 float32 number: %s" % sys.getsizeof(bf32))
print("size of 5 float32 numbers: %s" % sys.getsizeof(cf32), end='\n\n')

print("size of 0 float64 number: %s" % sys.getsizeof(af64))
print("size of 1 float64 number: %s" % sys.getsizeof(bf64))
print("size of 5 float64 numbers: %s" % sys.getsizeof(cf64), end='\n\n')

print("size of 0 chars: %s" % sys.getsizeof(''))
print("size of 1 chars: %s" % sys.getsizeof('1'))
print("size of 5 chars: %s" % sys.getsizeof('12345'), end='\n\n')
```

打印输出：

```
size of 0 int32 number: 96
size of 1 int32 number: 100
size of 5 int32 numbers: 116

size of 0 int64 number: 96
size of 1 int64 number: 104
size of 5 int64 numbers: 136

size of 0 float32 number: 96
size of 1 float32 number: 100
size of 5 float32 numbers: 116

size of 0 float64 number: 96
size of 1 float64 number: 104
size of 5 float64 numbers: 136

size of 0 chars: 49
size of 1 chars: 50
size of 5 chars: 54
```

# Reference

https://blog.csdn.net/u010099080/article/details/53411703?fps=1&locationNum=7
https://www.draketo.de/english/python-memory-numpy-list-array