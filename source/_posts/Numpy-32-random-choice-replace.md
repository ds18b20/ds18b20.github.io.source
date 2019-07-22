---
title: Numpy-32-random-choice-replace
date: 2018-12-26 22:08:21
categories: Python
tags:
- Numpy
- choice
- replace

---

# numpy.random.choice

`numpy.random.choice`(*a*, *size=None*, *replace=True*, *p=None*)

Generates a random sample from a given 1-D array

Parameters:	

**a** : 1-D array-like or int

If an ndarray, a random sample is generated from its elements. If an int, the random sample is generated as if a were np.arange(a)

**size** : int or tuple of ints, optional

Output shape. If the given shape is, e.g., `(m, n, k)`, then `m * n * k` samples are drawn. Default is None, in which case a single value is returned.

**replace** : boolean, optional

Whether the sample is with or without replacement

**p** : 1-D array-like, optional

The probabilities associated with each entry in a. If not given the sample assumes a uniform distribution over all entries in a.

其中replace参数表示，获得的随机序列是否可以包含重复元素。

具体为，

> **replace=True**: 可以从`a` 中反复选取同一个元素。
>
> **replace=False**: `a` 中同一个元素只能被选取一次。

# Reference

https://blog.csdn.net/silverdemon/article/details/77596343