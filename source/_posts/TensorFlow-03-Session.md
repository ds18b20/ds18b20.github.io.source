---
title: TensorFlow-03-Session
date: 2018-04-08 13:33:40
categories: TensorFlow
tags:
- Session
---

# 计算图

无论是TensorFlow还是Theano都是“符号式”的库，这与传统的逐行编译/解释的编程方式不同，符号式的编程首先定义好各种变量从而建立一个“计算图”，计算图规定了各个变量之间的计算关系。建立好的运算图需要compile以确定其内部细节。即使compile之后，计算图内仍然没有用于处理的数据，需要传入数据来形成数据流。

大多数的深度学习框架使用的都是符号计算这一套方法，因为符号计算能够提供关键的计算优化、自动求导等功能。

# Seesion会话

使用TF构建的计算图必须通过Seesion会话才能执行，只定义计算图而不执行Session会话的话，该节点的原酸不被执行。

例如，

```python
# -*- coding: utf-8 -*-
import tensorflow as tf

a=tf.constant([[1,2]])  #定义一个1×2的矩阵
b=tf.constant([[2],
               [4]])  #定义一个2×1的矩阵

c=tf.matmul(a,b)  #两者dot赋给c节点

sess=tf.Session()  # 定义一个Seesion实例
result=sess.run(c)  # 执行sess实例
print(result)

sess.close()  #关闭session，可有可无，但为了规范起见，还是写上
```

或者使用with上下文自动close

```python
with tf.Session() as sess:
    result=sess.run(c)
    print(result)
```

上述代码中，先定义了矩阵a和b，然后c = dot(a, b)，但是并没有开始计算c，而是等到Seesion处的run才开始运算。

#参考

一些基本概念-关于符号计算

http://keras-cn.readthedocs.io/en/latest/for_beginners/concepts/

tensorflow入门之Session

https://blog.csdn.net/zhangshaoxing1/article/details/68957564