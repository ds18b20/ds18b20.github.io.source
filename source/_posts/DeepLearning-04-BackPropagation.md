---
title: DeepLearning-04-BackPropogation
date: 2018-06-09 07:36:57
categories: DeepLearning
tags:
- Deep Learning
- Back Propagation

---

# 简介

深度学习算法主要包括两部分：

- 前向计算
- 反向传播

代入权重矩阵W和偏置矩阵b得到预测值$y=f(W,x)+b$ ，通过Loss函数得到当前预测的误差大小。反向计算$L=Loss(W,b)$得到误差值L对于权重矩阵和偏置矩阵b的梯度gradient，利用gradient来更新W和b使得误差值L逐步减小。

# Back Propagation

反向传播把复杂的运算转换成一个个简单的运算节点Dot。然后反向对各个参数求梯度时，整体的复杂求导运算就可以简化为各个节点Dot的链式求导。

## add

$y=a+b$，此时y处的梯度可以直接不加改变地直接传递到a和b上。

## mul
