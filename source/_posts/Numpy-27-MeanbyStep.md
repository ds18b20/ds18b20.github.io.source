---
title: Numpy-27-MeanbyStep
date: 2018-07-15 23:36:37
categories: Python
tags:
- Numpy
- mean
- step

---

# average an array by step

如果要每隔一个固定长度计算一个序列的平均数，可以考虑把这个序列按要求长度分割，然后求平均数。

使用Numpy时可以通过reshape方法变换维度，然后在行或者列内求平均即可。如下，

```python
np.mean(arr.reshape(-1, STEP), axis=1)
```

# Reference

[Averaging over every n elements of a numpy array](https://stackoverflow.com/questions/15956309/averaging-over-every-n-elements-of-a-numpy-array)

