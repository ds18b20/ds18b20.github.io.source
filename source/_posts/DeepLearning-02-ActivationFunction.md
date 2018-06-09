---
title: DeepLearning-02-ActivationFunction
date: 2018-04-22 10:55:34
categories: DeepLearning
tags:
- Deep Learning
- activation function

---

# 激活函数



关于输出层的激活函数，依据实现目的有多种选择。

当神经网络用于二分类问题时，输出层使用Step函数。

用于多分类问题时候，输出层使用Softmax激活函数。

用于回归问题时，输出层采用恒等函数，即不做输出变换直接输出本身。



分类问题：对于离散量的预测输出，如明天是否下雨；

回归问题：对于连续量的预测输出，如明天的气温或者降水概率。

