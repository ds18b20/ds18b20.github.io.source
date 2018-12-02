---
title: TensorFlow-12-variable
date: 2018-05-20 19:08:30
categories: TensorFlow
tags:
- variable
---

# variable in tensorflow

变量是一种特殊的张量的存在，用于暂存运算的状态。变量在TF中要**显式地**定义才能正确地当作变量而不是普通的张量来处理。

```python
import tensorflow as tf
import numpy as np

# basic
state = tf.Variable(0, name='state')
# print(state.name)
one = tf.constant(1, name='one_con')
# update = tf.assign_add(state, one, name='assign_add')
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

# tf.assign & tf.assign_add

tf.assign用于给Variable赋值，

```python
tf.assign(
    ref,
    value,
    validate_shape=None,
    use_locking=None,
    name=None
)
```

tf.assign_add可以对已存在的Variable做加法运算，此运算也可以直接使用`+`实现，

```python
w = tf.Variable(np.arange(12).reshape(3, 4), name='w', dtype=tf.float32)

init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print('w:', sess.run(w))

    a = w + 1.0
    print('a:', sess.run(a))
```

输出，

```
w: [[ 0.  1.  2.  3.]
 [ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
a: [[ 1.  2.  3.  4.]
 [ 5.  6.  7.  8.]
 [ 9. 10. 11. 12.]]
```

或者，使用tf.assign_add时

```python
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print('w:', sess.run(w))

    # a = tf.assign_add(w, 1.0)  # 注意这里会报错，提示形状不匹配
    a = w.assign_add(np.ones((3, 4)))
    print('a:', sess.run(a))
```

输出，

```
w: [[ 0.  1.  2.  3.]
 [ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
a: [[ 1.  2.  3.  4.]
 [ 5.  6.  7.  8.]
 [ 9. 10. 11. 12.]]
```

# tf.Variable &  tf.get_variable

## 定义

1. tf.Variable()

   ```python
   W = tf.Variable(<initial-value>, name=<optional-name>)
   ```

   用于生成一个初始值为initial-value的变量。必须指定初始化值

2. tf.get_variable()

   ```python
   W = tf.get_variable(name, shape=None, dtype=tf.float32, initializer=None, regularizer=None, trainable=True, collections=None)
   ```

   获取已存在的变量（要求不仅名字，而且初始化方法等各个参数都一样），如果不存在，就新建一个。 

## 区别

相比于tf.Variable，tf.get_variable会检查当前命名空间内是否存在相同name属性的variable，如果存在则检查是否设置为变量共享，如果没有设置则会抛出错误提示name重复。

```python
w = tf.Variable(np.arange(12).reshape(3, 4), name='w', dtype=tf.float32)
new_w = tf.Variable(np.arange(12).reshape(3, 4), name='w', dtype=tf.float32)

init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print('w:', sess.run(w))
    print('new_w:', sess.run(new_w))
```

输出，

```
w: [[ 0.  1.  2.  3.]
 [ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
new_w: [[ 0.  1.  2.  3.]
 [ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
```

相比之下，

```python
b = tf.get_variable(name='bias', shape=(3,), dtype=tf.float32)
new_b = tf.get_variable(name='bias', shape=(3,), dtype=tf.float32)
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print('b:', sess.run(b))
    print('new_b:', sess.run(new_b))
```

则会报错，提示name重复，是否要共享变量？

```
ValueError: Variable bias already exists, disallowed. Did you mean to set reuse=True or reuse=tf.AUTO_REUSE in VarScope? 
```

综上，应该优先选择使用tf.get_variable方法来避免name重复导致的一些意料之外的bug。

## tf.get_variable的应用

为了避免上述错误，有两种应用方式：

1. 使用`tf.variable_scope()`来分隔命名空间；
2. 使用`reuse=True`参数来让相同name的variable实现共享

### tf.variable_scope()添加命名空间

```python
with tf.variable_scope("foo"):
    v = tf.get_variable("v", [1]) #v.name == "foo/v:0"
```

就是给variable外面套上一个level更高的name。

### reuse=True共享变量

```python
with tf.variable_scope("one"):
    a = tf.get_variable("v", [1])  # a.name == "one/v:0"
with tf.variable_scope("one"):
    b = tf.get_variable("v", [1])  # 创建两个名字一样的变量会报错 
```

在相同的命名空间`one`内存在两个`v`变量，导致报错`ValueError: Variable one/v already exists` 。

```python
with tf.variable_scope("one"):
    a = tf.get_variable("v", [1])  # a.name == "one/v:0"
with tf.variable_scope("one", reuse = True):  # 注意reuse的作用。
    c = tf.get_variable("v", [1])  # c.name == "one/v:0" 成功共享，因为设置了reuse

assert a == c  # Assertion is true, they refer to the same object.
```

再看一下tf.Variable方法，

```
with tf.variable_scope("two"):
    d = tf.get_variable("v", [1])  # d.name == "two/v:0"
    e = tf.Variable(1, name="v", expected_shape=[1])  # e.name == "two/v_1:0"

assert d == e  # AssertionError: they are different objects
```

提示： `AssertionError`

在命名空间`two`内，在定义v之后，又使用tf.Variable来定义了相同名字`v`的变量。由于tf.Variable不会检查命名重复，所以后面定义的v会被系统自动修改为`v_1`。

https://blog.csdn.net/u012223913/article/details/78533910

# tf.variable_scope和tf.name_scope

tf.variable_scope

可以分隔命名空间，对tf.get_variable得到的变量有效，对tf.Variable的变量也有效。

tf.name_scope

可以分隔命名空间，对tf.get_variable得到的变量`无效`，对tf.Variable的变量有效。

```python
with tf.variable_scope('V1'):
    a1 = tf.get_variable(name='a1', shape=[1], initializer=tf.constant_initializer(1))
    a2 = tf.Variable(tf.random_normal(shape=[2, 3], mean=0, stddev=1), name='a2')
with tf.variable_scope('V2'):
    a3 = tf.get_variable(name='a1', shape=[1], initializer=tf.constant_initializer(1))
    a4 = tf.Variable(tf.random_normal(shape=[2, 3], mean=0, stddev=1), name='a2')

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(a1.name)
    print(a2.name)
    print(a3.name)
    print(a4.name)
```

输出，

```
V1/a1:0
V1/a2:0
V2/a1:0
V2/a2:0
```

使用tf.name_scope，

```python
with tf.name_scope('V1'):  # 使用tf.name_scope
    a1 = tf.get_variable(name='a1', shape=[1], initializer=tf.constant_initializer(1))
    a2 = tf.Variable(tf.random_normal(shape=[2, 3], mean=0, stddev=1), name='a2')
with tf.name_scope('V2'):  # 使用tf.name_scope
    a3 = tf.get_variable(name='a1', shape=[1], initializer=tf.constant_initializer(1))
    a4 = tf.Variable(tf.random_normal(shape=[2, 3], mean=0, stddev=1), name='a2')

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(a1.name)
    print(a2.name)
    print(a3.name)
    print(a4.name)
```

输出报错，

```
ValueError: Variable a1 already exists, disallowed. Did you mean to set reuse=True or reuse=tf.AUTO_REUSE in VarScope? 
```

https://blog.csdn.net/UESTC_C2_403/article/details/72328815

# Reference

关于共享变量，参考：

https://www.tensorflow.org/guide/variables?hl=zh-cn

http://jermmy.xyz/2017/08/25/2017-8-25-learn-tensorflow-shared-variables/

https://www.cnblogs.com/Charles-Wan/p/6200446.html

