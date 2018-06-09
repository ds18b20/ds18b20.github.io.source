---
title: Numpy-03-broadcasting
date: 2017-11-11 15:04:25
categories: Python
tags:
- Numpy
- Python
- broadcasting
---

# Numpy Broadcasting

## Inroduction

`The term broadcasting describes how numpy treats arrays with different shapes during arithmetic operations. Subject to certain constraints, the smaller array is “broadcast” across the larger array so that they have compatible shapes.`

Nmpy的broadcasting功能应用在两个ndarray的element-wise运算的前提之下。broadcasting功能使得“不同维度”的两个ndarray可以进行shape匹配。但是，这个“不同维度”也需要其中一个维度是1。

综上，ndarray的element-wise运算（+,-,*,/）要求为以下两种情况之一：

*a.完全相同shape*

*b.其中一个维度是1*

```python
>>> a=np.arange(3).reshape(3,1)
>>> a
array([[0],
       [1],
       [2]])
>>> a.shape
(3, 1)
>>> b=np.array([1,2])
>>> b
array([1, 2])
>>> b.shape
(2,)
>>> a+b
array([[1, 2],
       [2, 3],
       [3, 4]])
```

此运算相当于：

```python
>>> a
array([[0],
       [1],
       [2]])
# 变为：
>>> a
array([[0, 0],
       [1, 1],
       [2, 2]])
```

```python
>>> b
array([1, 2])
# 变为：
>>> b
array([[1, 2],
       [1, 2],
       [1, 2]])
```

```python
>>> a+b
array([[1, 2],
       [2, 3],
       [3, 4]])
```

当不满足上述shape要求时，python解释器会返回错误信息：

```python
ValueError: operands could not be broadcast together with shapes (8,4,3) (2,1)
```

## 具体理解

有两个矩阵A和B，A为4维矩阵，B为三维矩阵。

```
A      (4d array):  8 x 1 x 6 x 1
B      (3d array):      7 x 1 x 5
Result (4d array):  8 x 7 x 6 x 5
```

如果可以进行element-wise运算，则要求各个维度位置上，要么大小相同，要么其中之一为1。上面的例子中7 x 1 x 5的7的左侧没有维度，这是可以的，但是7和5之间的1如果是空位的话则不可以broadcast。7 x 5时必须通过np.reshape方法来转换成7 x 1 x 5维度。

```python
>>> import numpy as np
>>> a=np.array([[1,2,3],[4,5,6]])
>>> b=np.array([[1,1,1],[2,2,2],[3,3,3]])
>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> b
array([[1, 1, 1],
       [2, 2, 2],
       [3, 3, 3]])
>>> a-b
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: operands could not be broadcast together with shapes (2,3) (3,3)
>>> aa=a.reshape(2,1,3)
>>> aa-b
array([[[ 0,  1,  2],
        [-1,  0,  1],
        [-2, -1,  0]],

       [[ 3,  4,  5],
        [ 2,  3,  4],
        [ 1,  2,  3]]])
```

## 矩阵乘法

在进行矩阵的乘法（点乘）时，并没有上文所述element-wise运算时的broadcasting功能。

例如**A·B**如果不满足**A**.shape[1] == **B**.shape[0]，则会返回错误信息：

```python
ValueError: shapes (3,1) and (3,3) not aligned: 1 (dim 1) != 3 (dim 0)
```

另外，值得注意的是，当矩阵和一维数组进行矩阵乘法时，一维数组参与运算的方式随着其出现的位置而变化：

- 一维数组出现在矩阵**左侧**时，作为**行向量**使用
- 一维数组出现在矩阵**右侧**时，作为**列向量**使用

这就使得运算结果始终为一维数组。

```python
>>> b
array([1, 2])
>>> b.shape
(2,)
>>> b.T
array([1, 2])
>>> b.T.shape
(2,)
```

# 参考

[Python库numpy中的Broadcasting机制解析](https://blog.csdn.net/hongxingabc/article/details/53149655)