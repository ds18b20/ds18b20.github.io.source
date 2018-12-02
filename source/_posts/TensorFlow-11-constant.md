---
title: TensorFlow-11-constant
date: 2018-05-20 18:46:20
categories: TensorFlow
tags:
- constant

---

# constant in tensorflow

定义一个constant最简单的方式就是调用tf.constant方法

```python
import tensorflow as tf
import numpy as np

# basic
a = tf.constant(2, name='ax')
b = tf.constant(3, name='bx')
x = tf.add(a, b, name='add')
```

还有类似Numpy的zeros/zeros_like和ones/ones_like的方式生成全0或全1的张量

```python
# zeros/zeros_like
z = tf.zeros([2, 3], dtype=tf.int32, name='z')
zl = tf.zeros_like(np.arange(6).reshape(2, 3), dtype=tf.int32, name='zl')
zz = tf.multiply(z, zl, name='multiply')

# ones/ones_like
o = tf.ones([2, 3], dtype=tf.int32, name='o')
ol = tf.ones_like(np.arange(6).reshape(2, 3), dtype=tf.int32, name='ol')
```

还可以使用tf.fill向指定形状的张量内填入数据

```python
# fill array with a value
f = tf.fill(dims=[2, 3], value=6, name='f')
```

生成等距张量类似Numpy的range和linspace

```python
# sequences
# start must be float
# [s, e] both s and e are included!!!
ls = tf.linspace(start=1.0, stop=10, num=10, name='ls')
rg = tf.range(3, name='rg') # [0, 1, 2] same to Numpy
```

生成随机张量

```python
# random
rn = tf.random_normal(shape=[2, 3], mean=1.0, stddev=1.0, dtype=tf.float32, name='rn')
```

关于随机张量，还有以下多种生成函数：

- [`tf.random_normal`](https://www.tensorflow.org/api_docs/python/tf/random_normal)
- [`tf.truncated_normal`](https://www.tensorflow.org/api_docs/python/tf/truncated_normal)
- [`tf.random_uniform`](https://www.tensorflow.org/api_docs/python/tf/random_uniform)
- [`tf.random_shuffle`](https://www.tensorflow.org/api_docs/python/tf/random_shuffle)
- [`tf.random_crop`](https://www.tensorflow.org/api_docs/python/tf/random_crop)
- [`tf.multinomial`](https://www.tensorflow.org/api_docs/python/tf/multinomial)
- [`tf.random_gamma`](https://www.tensorflow.org/api_docs/python/tf/random_gamma)
- [`tf.set_random_seed`](https://www.tensorflow.org/api_docs/python/tf/set_random_seed)

# Reference

https://www.tensorflow.org/api_guides/python/constant_op

