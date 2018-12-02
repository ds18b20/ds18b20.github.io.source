---
title: TensorFlow-18-dropout
date: 2018-05-26 16:25:11
categories: TensorFlow
tags:
- dropout
- sklearn
- dataset
---

# sklearn dataset

以下代码展示了sklearn库自带的dataset的内容，

```python
import tensorflow as tf
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelBinarizer
import numpy as np
import matplotlib.pyplot as plt


# load data
digits = load_digits()
X = digits.data
y = digits.target

print(X.shape)
print(y.shape)

# show full dataset
plt.figure(figsize=(5, 1))
for i in range(5):
    plt.subplot(1, 5, i+1)
    plt.imshow(X[i].reshape(8, 8))
    plt.title(u"Digit: %s" % y[i])
plt.subplots_adjust(hspace=1, wspace=1)
plt.show()

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

# show train dataset
plt.figure(figsize=(5, 1))
for i in range(5):
    plt.subplot(1, 5, i+1)
    plt.imshow(X_train[i].reshape(8, 8))
    plt.title(u"Digit: %s" % y_train[i])
plt.subplots_adjust(hspace=1, wspace=1)
plt.show()

# show test dataset
plt.figure(figsize=(5, 1))
for i in range(5):
    plt.subplot(1, 5, i+1)
    plt.imshow(X_test[i].reshape(8, 8))
    plt.title(u"Digit: %s" % y_test[i])
plt.subplots_adjust(hspace=1, wspace=1)
plt.show()
```

输出的X和y的尺寸分别为，`(1797, 64)`和`(1797,)`

可知，总共1797个样本，每个样本包括image部分和对应的label。

image是被flaten的1\*64的行向量，要显示的话需要reshape成8\*8。label是1797ge元素组成的一维向量。

输出的图像如下，首先是整体样本的前5个样本：

![](TensorFlow-18-dropout\dataset_1.png)

然后是split得到的train和test的前5个样本：

![](TensorFlow-18-dropout\dataset_2.png)

![](TensorFlow-18-dropout\dataset_3.png)

可以看到，得到的train和test样本已经得到了shaffle。

# dropout

神经网络由于配置不当会导致`过拟合`(over fitting)，即训练得到的Weights和biases对训练样本适应的很好(过好)，但是对测试样本适应得不好的现象。出现over fitting之后神经网络的通用性降低，当应用于具体实例时不能准确预测。



```python
import tensorflow as tf
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelBinarizer
import numpy as np
import matplotlib.pyplot as plt


# load data
digits = load_digits()
X = digits.data
y = digits.target
y = LabelBinarizer().fit_transform(y)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)


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


# real data for placeholder

# placeholder for inputs data
with tf.name_scope('inputs'):
    xs = tf.placeholder(dtype=tf.float32, shape=[None, 64], name='x_inputs')
    ys = tf.placeholder(dtype=tf.float32, shape=[None, 10], name='y_inputs')

# add hidden layer
layer_1 = add_layer(xs, 64, 100, 'hidden_layer_1', activation_function=tf.nn.tanh)
# add prediction layer
prediction = add_layer(layer_1, 100, 10, 'prediction_layer', activation_function=tf.nn.softmax)

# loss function
with tf.name_scope('loss'):
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(ys * tf.log(prediction), reduction_indices=[1]))
    tf.summary.scalar('loss', cross_entropy)
# training
with tf.name_scope('train'):
    train_step = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cross_entropy)

# initialize
init = tf.global_variables_initializer()

# Session
sess = tf.Session()
sess.run(init)
summaries = tf.summary.merge_all()
train_writer = tf.summary.FileWriter('./graphs/dropout/train', sess.graph)
test_writer = tf.summary.FileWriter('./graphs/dropout/test', sess.graph)

# start training
for step in range(1000):
    sess.run(train_step, feed_dict={xs: X_train, ys: y_train})
    if step % 50 == 0:
        # print(compute_accuracy(mnist.test.images, mnist.test.labels))
        train_result = sess.run(summaries, feed_dict={xs: X_train, ys: y_train})
        test_result = sess.run(summaries, feed_dict={xs: X_test, ys: y_test})
        train_writer.add_summary(train_result, global_step=step)
        test_writer.add_summary(test_result, global_step=step)
print('OK')
```

上例中给中间层指定了100个节点，有意让其发生`过拟合`，而且没有加入dropout层。结果如下，

![](TensorFlow-18-dropout\dropout_1.JPG)

可以看到，随着学习步数的增加，train和test的loss差距开始变大，这就意味着该W和b对训练集有很好的预测，而对测试集适应性不佳。

为了解决over fitting，可以给网络中添加drop out层，即人为地切断层中的某一些连接，从而抑制W和b过于适应于训练样本。

```python
...
def add_layer(inputs, in_size, out_size, layer_name, activation_function=None):
    ...
	with tf.name_scope('W_m_p'):
		W_m_p = tf.matmul(inputs, Weights) + biases
	with tf.name_scope('Dropout'):
		W_m_p = tf.nn.dropout(W_m_p, keep_prob)
...
```

以及

```python
# placeholder for inputs data
with tf.name_scope('inputs'):
	# keep_prob代表保留比例
    keep_prob = tf.placeholder(dtype=tf.float32, name='keep_prob4dropout')
    xs = tf.placeholder(dtype=tf.float32, shape=[None, 64], name='x_inputs')
    ys = tf.placeholder(dtype=tf.float32, shape=[None, 10], name='y_inputs')
```

train的时候给keep_prob这个placeholder赋值0.6，生成Summary的时候赋1即可。

```python
# start training
for step in range(1000):
    sess.run(train_step, feed_dict={xs: X_train, ys: y_train, keep_prob: 0.6})
    if step % 50 == 0:
        # print(compute_accuracy(mnist.test.images, mnist.test.labels))
        train_result = sess.run(summaries, feed_dict={xs: X_train, ys: y_train, keep_prob: 1})
        test_result = sess.run(summaries, feed_dict={xs: X_test, ys: y_test, keep_prob: 1})
        train_writer.add_summary(train_result, global_step=step)
        test_writer.add_summary(test_result, global_step=step)
print('OK')
```

生成的计算图中也就新增加了dropout层，如下：

![](TensorFlow-18-dropout\dropout_2.png)

此时，loss输出如下：

![](TensorFlow-18-dropout\dropout_3.JPG)

同dropout前相比，over fitting得到大幅改善。

**注意**：运行时可能会报错`Nan in summary histogram for: `，参见文章`TensorFlow-00-Error`来调整超参数。

完整代码参见我的[Github](https://github.com/ds18b20/TensorFlow/blob/master/tutorial/08-dropout.py)

