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

