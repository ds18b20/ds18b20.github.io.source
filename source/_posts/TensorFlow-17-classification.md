---
title: TensorFlow-17-classification
date: 2018-05-25 00:02:55
categories: TensorFlow
tags:
- tensorboard
- classification
---

# classification

之前的文章中一直使用的模拟一个二次函数的例子，其实是为了解决一个回归问题(regression)，接下来看一下分类(classification)相关的例子：

```python
import tensorflow as tf
import numpy as np


from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)


def add_layer(inputs, in_size, out_size, layer_name, activation_function=None):
    with tf.name_scope('layer'):
        with tf.name_scope('Weights'):
            Weights = tf.Variable(tf.random_normal([in_size, out_size]), name='W')
            tf.summary.histogram(layer_name + '/Weights', Weights)
        with tf.name_scope('biases'):
            biases = tf.Variable(tf.zeros([1, out_size]) + 0.1, name='b')
            tf.summary.histogram(layer_name + '/biases', biases)
        with tf.name_scope('W_m_p'):
            W_m_p = tf.matmul(inputs, Weights) + biases
        if activation_function is None:
            outputs = W_m_p
        else:
            outputs = activation_function(W_m_p)
            tf.summary.histogram(layer_name + '/outputs', outputs)
        return outputs


def compute_accuracy(xs_val, ys_val):
    global prediction
    y_pre = sess.run(prediction, feed_dict={xs: xs_val})
    correct_prediction = tf.equal(tf.argmax(y_pre, axis=1), tf.argmax(ys_val, axis=1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    result = sess.run(accuracy, feed_dict={xs: xs_val, ys: ys_val})

    return result


# real data for placeholder

# placeholder for inputs data
with tf.name_scope('inputs'):
    xs = tf.placeholder(dtype=tf.float32, shape=[None, 28*28], name='x_inputs')
    ys = tf.placeholder(dtype=tf.float32, shape=[None, 10], name='y_inputs')

# add hidden layer

# add prediction layer
prediction = add_layer(xs, 28*28, 10, 'prediction_layer', activation_function=tf.nn.softmax)

# loss function
with tf.name_scope('loss'):
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(ys * tf.log(prediction), reduction_indices=[1]))
    tf.summary.scalar('loss', cross_entropy)
# training
with tf.name_scope('train'):
    train_step = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cross_entropy)

# initialize
init = tf.global_variables_initializer()

sess = tf.Session()

sess.run(init)
summaries = tf.summary.merge_all()
writer = tf.summary.FileWriter('./graphs/classification', sess.graph)

# start training
for step in range(1000):
    xs_batch, ys_batch = mnist.train.next_batch(100)
    sess.run(train_step, feed_dict={xs: xs_batch, ys: ys_batch})
    if step % 50 == 0:
        print(compute_accuracy(mnist.test.images, mnist.test.labels))
        summary_result = sess.run(summaries, feed_dict={xs: xs_batch, ys: ys_batch})
        writer.add_summary(summary_result, global_step=step)
print('OK')
```

输出，

```
0.1332
0.3288
0.4689
...
0.8005
0.8045
OK
```

![](TensorFlow-17-classification\classification.JPG)

# tf.cast

tf.cast(x, dtype, name=None)  将输入x转化成dtype类型。例如，原来x的数据格式是bool，那么将其转化成float以后，就能够将其转化成0和1的序列。