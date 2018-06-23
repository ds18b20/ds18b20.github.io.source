---
title: DeepLearning-05-Regression
date: 2018-06-16 21:33:56
categories: DeepLearning
tags:
- Deep Learning
- Regression

---

# 单层神经网络

单层神经网络是一个简单的神经网络模型，主要有线性回归和Softmax回归两种形式。前者用于对连续值的预测，例如价格，温度等。后者主要用于对离散值的预测，比如图片分类。本文主要是针对前者进行分析和实现。

# 线性回归的基本要素

下面以一个有两个自变量的例子来解释线性回归的基本要素。

## 模型

设模型有两个输入量$x_1$和$x_2$，称为特征，对应的预测量为$y$。这里规定样本标签表示为${y}$，而根据模型的预测值为$\hat{y}$。则有，
$$
\hat{y} = x_1 w_1 + x_2 w_2 + b
$$
其中$w_1, w_2$是权重项(weight)，b是偏置项(bias)。

## 训练数据

训练数据集由多个数据样本组成，每个样本包括**输入特征**和实际对应的**标签**。

## 误差函数

误差函数(损失函数)有多种，线性回归模型下最常用的是平方差误差(square loss)，其定义如下：
$$
\ell^{(i)}(w_1, w_2, b) = \frac{(\hat{y}^{(i)} - y^{(i)})^2}{2}
$$
当损失最小时，模型有最优秀的预测能力。所以对模型的训练归结为求损失函数最小值的问题。
$$
\ell(w_1, w_2, b) =\frac{1}{n} \sum_{i=1}^n \ell^{(i)}(w_1, w_2, b) =\frac{1}{n} \sum_{i=1}^n \frac{(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)})^2}{2}
$$
其中n表示样本数，或者是mini batch的样本数。除以n可以看成在训练集上的平均误差。

## 优化函数

一般来讲，对一个函数求最小值的，可以转化为求解损失函数的微分函数的零点的问题。但是对于复杂的问题，求微分函数本身比较苦难时，需要一中更加通用的方法，这里就可以使用梯度下降法。针对在时间中遇到的各种问题，梯度下降法也演化出各种版本，比如最常用的小批量随机梯度下降法(mini-batch stochastic gradient descent )。

具体实现为，

- 首先随机从样本总体中选取一个小批量样本(mini batch)$\mathcal{B}$
- 然后计算小批量样本的平均损失函数对于模型参数的梯度
- 最后对模型参数按照梯度的反方向调整(按比例)

将上述过程进行有限次迭代即可求得最优解。
$$
w_1 \leftarrow w_1 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_1} = w_1 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}x_1^{(i)} (x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}),
$$

$$
w_2 \leftarrow w_2 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_2} = w_2 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}x_2^{(i)} (x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}),
$$

$$
b \leftarrow b -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial b} = b -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}).
$$

上面，$|\mathcal{B}|$表示表示小批量样本的大小(batch size)，$\eta$称为学习速率(learning rate)是个正数。这里的批量大小和学习速率是设为设定的参数，一旦确定之后，不会随着样本的迭代发生变化，因此被称为超参数(hyper parameter)。

# 具体实现

下面开始一个简单的单层全连接层(fully-connected layer)或者稠密层(dense layer)神经网络。

## 计算的矢量表达

用矢量方式进行计算，相比于多层循环下对元素的操作，有更快的执行速度，可以大幅削减训练时间。
$$
\begin{split}\hat{y}^{(1)} = x_1^{(1)} w_1 + x_2^{(1)} w_2 + b,\\
\hat{y}^{(2)} = x_1^{(2)} w_1 + x_2^{(2)} w_2 + b,\\
\hat{y}^{(3)} = x_1^{(3)} w_1 + x_2^{(3)} w_2 + b.\end{split}
$$
就可以直接转换成矩阵间的操作，
$$
\begin{split}\boldsymbol{\hat{y}} =
\begin{bmatrix}
    \hat{y}^{(1)} \\
    \hat{y}^{(2)} \\
    \hat{y}^{(3)}
\end{bmatrix},\quad
\boldsymbol{X} =
\begin{bmatrix}
    x_1^{(1)} & x_2^{(1)} \\
    x_1^{(2)} & x_2^{(2)} \\
    x_1^{(3)} & x_2^{(3)}
\end{bmatrix},\quad
\boldsymbol{w} =
\begin{bmatrix}
    w_1 \\
    w_2
\end{bmatrix}
\end{split}
$$
则有，
$$
\boldsymbol{\hat{y}} = \boldsymbol{X} \boldsymbol{w} + b
$$
其中模型输出$\boldsymbol{\hat{y}} \in \mathbb{R}^{n \times 1}$，批量特征$\boldsymbol{X} \in \mathbb{R}^{n \times d}$，权重$\boldsymbol{w} \in \mathbb{R}^{d \times 1}$，偏置$\boldsymbol{b} \in \mathbb{R}$。

相应的梯度下降可以写成，
$$
\boldsymbol{\theta} \leftarrow \boldsymbol{\theta} -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}   \nabla_{\boldsymbol{\theta}} \ell^{(i)}(\boldsymbol{\theta})
$$
其中，梯度的组成为，
$$
\begin{split}\nabla_{\boldsymbol{\theta}} \ell^{(i)}(\boldsymbol{\theta})=
\begin{bmatrix}
    \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_1} \\
    \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_2} \\
    \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial b}
\end{bmatrix}
=
\begin{bmatrix}
    x_1^{(i)} (x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}) \\
    x_2^{(i)} (x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}) \\
    x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}
\end{bmatrix}
\end{split}
$$

## 生成数据集

首先设定目标模型：

- 权重$\boldsymbol{w} = [2, 3]^\top$
- 偏差$b = 5$

即$y = 2*x_1 + 3*x_2 + 5$

现在为了模拟真实的样本，给目标模型加上随机误差项$\epsilon$，
$$
\boldsymbol{y} = \boldsymbol{X}\boldsymbol{w} + b + \epsilon
$$

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

num_inputs = 2
num_examples = 1000
true_w = [3, 7]
true_b = 5

features = np.random.randn(num_examples, num_inputs)
labels = np.dot(features, true_w)+ true_b
epsilon = np.random.normal(0.0, scale=0.01)
labels += epsilon

if __name__ == '__main__':
    fig = plt.figure(figsize=(5, 5), dpi=80)
    ax = plt.axes(projection='3d')

    ax.scatter(features[:, 0], features[:, 1], labels, c='blue')
    ax.set_xlabel('X Label')
    ax.set_ylabel('Y Label')
    ax.set_zlabel('Z Label')
    ax.set_title('scatter')

    plt.show()
```

得到，

![](DeepLearning-05-Regression\dataset.png)

## 抽取样本

定义一个返回`batch_size`个随机样本的特征和标签的函数。

```python
def update_batch(self, features_set, labels_set):
    # assert len(features_set) == len(labels_set)
    num_examples = len(features_set)
    index = np.random.choice(num_examples, self.__batch_size)
    self.__batch_features = features_set[index]
    self.__batch_labels = labels_set[index]
```

使用了numpy.random.choice()函数从样本总体中随机选取batch_size个样本。

## 前向计算

```python
def forward_pass(self):
    self.__batch_outputs = np.dot(self.__batch_features, self.__w) + self.__b  # (batch, )
```

上述代码实现了$\boldsymbol{\hat{y}} = \boldsymbol{X} \boldsymbol{w} + b$

## 反向传播

```python
def gradient_cal(self):
    # assert len(batch_features) == len(batch_labels)
    delta = self.__batch_outputs - self.__batch_labels  # (mini batch, )
    self.__grad_w = np.dot(delta, self.__batch_features) / self.__batch_size  # (batch, )dot(batch, vec_size)=(vec_size, )
    self.__grad_b = delta.sum() / self.__batch_size
```

注意batch计算处理的矩阵形状。

## 梯度下降

```python
def sgd(self, lr=0.01):
    self.__w -= lr * self.__grad_w
    self.__b -= lr * self.__grad_b
```

使用了最直接的**小批量随机梯度下降法**来更新参数。

# 代码示例

全部代码如下：

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D


class OneLayerRegression(object):
    def __init__(self, input_vec_size: int, batch_size: int=10):
        self.__input_vec_size = input_vec_size
        self.__batch_size = batch_size

        self.__batch_features = None
        self.__batch_labels = None
        
        self.__batch_outputs = None

        self.__w = np.random.normal(loc=0.0, scale=0.01, size=[2, ])
        self.__b = np.zeros(shape=[1, ])
        self.__grad_w = np.random.normal(loc=0.0, scale=0.01, size=[2, ])
        self.__grad_b = np.zeros(shape=[1, ])

    def set_w(self, value):
        self.__w = value

    def set_b(self, value):
        self.__b = value

    def get_w(self):
        return self.__w

    def get_b(self):
        return self.__b

    def generate_simple_dataset(self, num_examples):
        true_w = np.array([3.0, 7.0])
        true_b = np.array([5.0, ])

        features = np.random.randn(num_examples, self.__input_vec_size)
        labels = np.dot(features, true_w) + true_b
        epsilon = np.random.normal(0.0, scale=0.01)
        labels += epsilon

        return features, labels
    
    # linear regression
    def forward_pass(self):
        self.__batch_outputs = np.dot(self.__batch_features, self.__w) + self.__b  # (batch, )

    def squared_loss(self):
        return ((self.__batch_outputs - self.__batch_labels)**2).sum() / self.__batch_size

    def backward_pass(self):
        self.gradient_cal()
        self.sgd(lr=0.01)

    def gradient_cal(self):
        # assert len(batch_features) == len(batch_labels)
        delta = self.__batch_outputs - self.__batch_labels  # (mini batch, )
        self.__grad_w = np.dot(delta, self.__batch_features) / self.__batch_size  # (batch, )dot(batch, vec_size)=(vec_size, )
        self.__grad_b = delta.sum() / self.__batch_size

    def sgd(self, lr=0.01):
        self.__w -= lr * self.__grad_w
        self.__b -= lr * self.__grad_b

    def update_batch(self, features_set, labels_set):
        # assert len(features_set) == len(labels_set)
        num_examples = len(features_set)
        index = np.random.choice(num_examples, self.__batch_size)
        self.__batch_features = features_set[index]
        self.__batch_labels = labels_set[index]


if __name__ == '__main__':
    # fig = plt.figure(figsize=(5, 5), dpi=80)
    # ax = plt.axes(projection='3d')
    #
    # ax.scatter(features[:, 0], features[:, 1], labels, c='blue')
    # ax.set_xlabel('X Label')
    # ax.set_ylabel('Y Label')
    # ax.set_zlabel('Z Label')
    # ax.set_title('scatter')
    #
    # plt.show()
    reg = OneLayerRegression(input_vec_size=2, batch_size=10)
    sample_features, sample_labels = reg.generate_simple_dataset(num_examples=1000)

    loss = []
    for i in range(1000):
        reg.update_batch(sample_features, sample_labels)
        reg.forward_pass()
        reg.backward_pass()

        loss.append(reg.squared_loss())

    print(reg.get_w())
    print(reg.get_b())

    plt.plot(loss)
    plt.show()
```

得到训练后的模型参数为，

[2.99995686 6.99972363]
[5.01386146]

和预设值

```python
true_w = np.array([3.0, 7.0])
true_b = np.array([5.0, ])
```
的误差在允许范围内。同时loss的可视化效果为，

![](DeepLearning-05-Regression\loss.png)

# 参考

http://zh.gluon.ai/chapter_supervised-learning/shallow-model.html