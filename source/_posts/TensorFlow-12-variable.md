---
title: TensorFlow-12-variable
date: 2018-05-20 19:08:30
categories: TensorFlow
tags:
- variable
---

# variable in tensorflow

变量是一种特殊的张量的存在，用于暂存运算的状态。变量在TF中要显式地定义才能正确地当作变量而不是普通的张量来处理。

```python
import tensorflow as tf
import numpy as np

# basic
state = tf.Variable(0, name='state')
# print(state.name)
one = tf.constant(1, name='one_con')
new_value = tf.add(state, one, name='new_val')
update = tf.assign(state, new_value, name='assign')

init = tf.global_variables_initializer()

with tf.Session() as sess:
    # show in Tensorboard
    writer = tf.summary.FileWriter("./graphs", sess.graph)
    sess.run(init)
    for _ in range(3):
        print(sess.run(update))
```

输出结果：

1

2

3

可视化效果如下：

![](TensorFlow-12-variable\variable.png)