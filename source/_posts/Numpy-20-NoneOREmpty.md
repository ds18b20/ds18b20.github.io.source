---
title: Numpy-20-NoneOREmpty
date: 2018-04-28 22:51:10
categories: Python
tags:
- Numpy
- None
- Array
- size
---

# 判断一个对象是None还是Numpy Array

OpenCV的Hough直线检测等函数，返回有时候会是None对象。这时候进行下一步数据处理时就需要判断此返回值是None还是array。

最直接的想法就是判断是不是等于None：

```python
if a is None:
    ...
else:
    ...
```

或者使用type和isinstance判断：

```python
# be careful not to check for np.array but for np.ndarray!
if type(a) is np.ndarray:
    ...
else:
    ...
```

ininstance可以判断是不是一个类的子类，

```python
# be careful not to check for np.array but for np.ndarray!
if isinstance(a, np.ndarray):
    ...
else:
    ...   
```

# 判断Array是否为空

若已知是Numpy Array，要判断内容是否为空的话，可以查看a.size属性。

# 参考

ValueError when checking if variable is None or numpy.array

https://stackoverflow.com/questions/36783921/valueerror-when-checking-if-variable-is-none-or-numpy-array



How can I check whether the numpy array is empty or not?

https://stackoverflow.com/questions/11295609/how-can-i-check-whether-the-numpy-array-is-empty-or-not