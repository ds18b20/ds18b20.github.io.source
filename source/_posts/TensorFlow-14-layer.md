---
title: TensorFlow-14-layer
date: 2018-05-22 22:10:30
categories: TensorFlow
tags:
- layer
---

# add a layer to graph

下面的代码实现了一个简单的拥有一个隐藏层的神经网络，用以模拟一个二次函数：

```python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt


def add_layer(input_data, in_size, out_size, activator=None):
    weights = tf.Variable(tf.random_normal([in_size, out_size]))
    biases = tf.Variable(tf.zeros([1, out_size]))
    Affine = tf.add(tf.matmul(input_data, weights), biases)
    if activator is None:
        output_data = Affine
    else:
        output_data = activator(Affine)

    return output_data


# sample data
x_data = np.linspace(-1, 1, 300)[:, np.newaxis]
noise = np.random.normal(0, 0.05, x_data.shape)
y_data = np.square(x_data) - 0.5 + noise

xs = tf.placeholder(dtype=tf.float32, shape=[None, 1])
ys = tf.placeholder(dtype=tf.float32, shape=[None, 1])

layer_1 = add_layer(input_data=xs, in_size=1, out_size=10, activator=tf.nn.relu)
prediction = add_layer(input_data=layer_1, in_size=10, out_size=1, activator=None)

# loss function
loss = tf.reduce_mean(tf.square(ys - prediction))  # no reduction_indices selected
# loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction), reduction_indices=1))

# training
# train_step = tf.train.AdamOptimizer(learning_rate=1e-3).minimize(loss)  # Adam
train_step = tf.train.GradientDescentOptimizer(learning_rate=1e-3).minimize(loss)  # Gradient Descent
# initialize
init = tf.global_variables_initializer()

loss_lst =[]
N = 2000
epoch = 100
with tf.Session() as sess:
    sess.run(init)
    for i in range(N):
        sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
        ls = sess.run(loss, feed_dict={xs: x_data, ys: y_data})
        loss_lst.append(ls)
        if i % epoch == 0:
            print(ls)

    colors = np.random.rand(N)
    plt.scatter(range(N), loss_lst, c='grey',marker='X')
    plt.xlabel('training step')
    plt.ylabel('loss')
    plt.show()
```

拆开来分析，

```python
def add_layer(input_data, in_size, out_size, activator=None):
    weights = tf.Variable(tf.random_normal([in_size, out_size]))
    biases = tf.Variable(tf.zeros([1, out_size]))
    Affine = tf.add(tf.matmul(input_data, weights), biases)
    if activator is None:
        output_data = Affine
    else:
        output_data = activator(Affine)

    return output_data
```

上面这一段实现了一个层定义函数。

```python
# sample data
x_data = np.linspace(-1, 1, 300)[:, np.newaxis]
noise = np.random.normal(0, 0.05, x_data.shape)
y_data = np.square(x_data) - 0.5 + noise
```

上面这一段生成了一组x和y的样本，加入了random量。

```python
xs = tf.placeholder(dtype=tf.float32, shape=[None, 1])
ys = tf.placeholder(dtype=tf.float32, shape=[None, 1])
```

上面这一段定义了两个placeholder，shape=[None, 1]，表示输入和输出行数不限，列数为1。这样提高了模型的通用性，因为可以随意改变样本的数量，本例中可以把输入[300, 1]改成任意多行，比如[500, 1]。

```python
layer_1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
prediction = add_layer(layer_1, 10, 1, activation_function=None)
```

这一段利用前面定义的添加层函数实现了一个layer_1层和一个prediction层。由于是回归问题，prediction层不需要激活函数。

```python
# loss function
loss = tf.reduce_mean(tf.square(ys - prediction))  # no reduction_indices selected
# loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction), reduction_indices=1))

# training
# train_step = tf.train.AdamOptimizer(learning_rate=1e-3).minimize(loss)  # Adam
train_step = tf.train.GradientDescentOptimizer(learning_rate=1e-3).minimize(loss)  # Gradient Descent
# initialize
init = tf.global_variables_initializer()
```

这一段定义了loss函数和training函数，并且对变量做初始化。

```python
loss_lst =[]
N = 2000
epoch = 100
with tf.Session() as sess:
    sess.run(init)
    for i in range(N):
        sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
        ls = sess.run(loss, feed_dict={xs: x_data, ys: y_data})
        loss_lst.append(ls)
        if i % epoch == 0:
            print(ls)

    colors = np.random.rand(N)
    plt.scatter(range(N), loss_lst, c='grey',marker='X')
    plt.xlabel('training step')
    plt.ylabel('loss')
    plt.show()
```

训练N=100步，每隔100步输出一次loss。最后把所有loss放到一个list里，并用scatter描绘出来。

![](TensorFlow-14-layer\layer.png)

继续修改一下可视化的部分，使得训练过程的显示更加友好，

```python
# initialize
init = tf.global_variables_initializer()

loss_lst =[]
N = 100
fig = plt.figure()
ax_1 = fig.add_subplot(1, 2, 1)
ax_2 = fig.add_subplot(1, 2, 2)
ax_2.scatter(x_data, y_data)

with tf.Session() as sess:
    sess.run(init)
    for i in range(N):
        sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
        ls = sess.run(loss, feed_dict={xs: x_data, ys: y_data})
        loss_lst.append(ls)
        colors = np.random.rand(N)
        if i % 10 == 0:
            # print(ls)
            try:
                ax_2.lines.remove(lines[0])
            except Exception:
                pass
            prediction_val = sess.run(prediction, feed_dict={xs: x_data})
            # ax_2.scatter(x_data, prediction_val, c='black', marker='X')
            lines = ax_2.plot(x_data, prediction_val, c='black', marker='X')
            plt.pause(0.25)
    ax_1.scatter(range(N), loss_lst, c='grey', marker='X')

    plt.xlabel('training step')
    plt.ylabel('loss')
    plt.show()
```

下面左侧的图表示loss的变化过程，右侧动态显示prediction的变化过程，以下是最后的输出效果：

![](TensorFlow-14-layer\layer_1.png)