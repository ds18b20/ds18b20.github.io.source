---
title: TensorFlow-20-reduction_indices
date: 2018-11-17 17:51:21
categories: TensorFlow
tags:
- reduce_sum
- reduce_mean
- reduction_indices

---

# reduction_indices参数

使用TF的reduce_sum和reduce_mean方法时，参数中有一项reduction_indices。

这个参数类似于Numpy中的axis参数，用于指定计算所在的维度。

![](TensorFlow-20-reduction-indices\reduction indices.png)

不指定的时候，默认值为None，意为在所有维度上计算，即压缩到0维也就是一个数。

# Reference

http://www.cnblogs.com/likethanlove/p/6547405.html