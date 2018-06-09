---
title: TensorFlow-10-assign
date: 2018-05-13 23:05:19
categories: TensorFlow
tags:
- assign
---

# assign

向一个变量赋值的操作方法：

```python
import tensorflow as tf

x = tf.Variable(0)
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(sess.run(x, feed_dict={x: 3}))
    print(sess.run(x))
```

输出为，`3和0`

注意到，x并没有真正赋值给x，仅仅为了评估而临时赋值给x。

真正的赋值需要tf.assign方法：

```python
x = tf.Variable(0)
y = tf.assign(x, 1)
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(sess.run(x))
    print(sess.run(y))
    print(sess.run(x))
```

输出为，`0，1和1`

# 参考

[How to assign a value to a TensorFlow variable?](https://stackoverflow.com/questions/34220532/how-to-assign-a-value-to-a-tensorflow-variable)

