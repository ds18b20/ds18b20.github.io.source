---
title: Numpy-05-log
date: 2017-11-15 19:06:54
categories: Python
tags:
- Numpy
- Python
- log

---

# Numpy.log

## numpy.log

Natural logarithm, **element-wise**.

```python
>>> np.log(np.e)
1.0
```

## numpy.log2

Base-2 logarithm of *x*.

```python
>>> np.log2(2)
1.0
```

## numpy.log10

Return the base 10 logarithm of the input array, element-wise.

```python
>>> np.log10(10)
1.0
```

# ERROR: “Divide by zero encountered in log” when not dividing by zero

由于log函数$y = log(x)$是一条单调递增曲线，且在x=0处y取值为负无穷。所以在计算机中计算log时，要注意避免x=0。
计算时可以在x的基础上添加一个微笑的delta，即log(x+delta)。

# 参考

[“Divide by zero encountered in log” when not dividing by zero](https://stackoverflow.com/questions/36229340/divide-by-zero-encountered-in-log-when-not-dividing-by-zero/36229376)

