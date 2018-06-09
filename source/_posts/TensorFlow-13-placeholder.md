---
title: TensorFlow-13-placeholder
date: 2018-05-20 22:24:32
categories: TensorFlow
tags:
- placeholder

---

# placeholder in tensorflow

placeholder先不定义数据的参数（尺寸等），但承诺后期会提供。

```python
import tensorflow as tf
import numpy as np

# basic
input_1 = tf.placeholder(tf.float32, name='input_1')
input_2 = tf.placeholder(tf.float32, name='input_2')

output = tf.multiply(input_1, input_2, name='multiply')

with tf.Session() as sess:
    # show in Tensorboard
    writer = tf.summary.FileWriter("./graphs", sess.graph)
    print(sess.run(output, feed_dict={input_1: [3.], input_2: [2.]}))
```

输出：

[6.]

可视化输出为：

![](TensorFlow-13-placeholder\placeholder.png)