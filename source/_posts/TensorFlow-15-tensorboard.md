---
title: TensorFlow-15-tensorboard
date: 2018-05-23 20:50:18
categories: TensorFlow
tags:
- tensorboard
---

# Visualization by tensorboard

```python
import tensorflow as tf
import numpy as np


def add_layer(inputs, in_size, out_size, activation_function=None):
    with tf.name_scope('layer'):
        with tf.name_scope('Weights'):
            Weights = tf.Variable(tf.random_normal([in_size, out_size]), name = 'W')
        with tf.name_scope('biases'):
            biases = tf.Variable(tf.zeros([1, out_size]) + 0.1, name='b')
        with tf.name_scope('W_m_p'):
            W_m_p = tf.matmul(inputs, Weights) + biases
        if activation_function is None:
            outputs = W_m_p
        else:
            outputs = activation_function(W_m_p)

        return outputs


x_data = np.linspace(-1, 1, 300)[:, np.newaxis]
noise = np.random.normal(0, 0.05, x_data.shape)
y_data = np.square(x_data) - 0.5 + noise

# placeholder for inputs data
with tf.name_scope('inputs'):
    xs = tf.placeholder(dtype=tf.float32, shape=[None, 1], name='x_inputs')
    ys = tf.placeholder(dtype=tf.float32, shape=[None, 1], name='y_inputs')

# add hidden layer
layer_1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
# add prediction layer
prediction = add_layer(layer_1, 10, 1, activation_function=None)
# loss function
with tf.name_scope('loss'):
    loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction), reduction_indices=[1]))
# training
with tf.name_scope('train'):
    train_step = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(loss)
# initialize
init = tf.global_variables_initializer()

with tf.Session() as sess:
    sess.run(init)
    write = tf.summary.FileWriter('./graphs', sess.graph)
```

在浏览器中显示的效果如下：

![](TensorFlow-15-tensorboard\graph.png)

双击可以打开各层：

![](TensorFlow-15-tensorboard\graph_large.png)

使用`tf.name_scope('name')`来构建比较大的计算图块，比如上面例子中的inputs、loss块等。

各计算节点内部可以通过加入`name='node_name'`参数来精细化地命名，比如上面例子中的`name = 'W'`。

要输出计算图，首先要初始化一个Session，在此Session下执行`tf.summary.FileWriter('./graphs', sess.graph)`可以把前面定义的图输出到`./graphs`目录下。

要启动tensorboard，打开终端，输入`tensorboard --logdir 记录文件所在目录`。然后在浏览器内按照命令行的提示输入local地址即可显示。

# sub folder

`tf.summary.FileWriter('./graphs', sess.graph)`

可以在log目录下继续添加sub folder来给事件日志分类，这样在浏览器中打开后，左上方Run处可以选择某一个sub folder来显示。