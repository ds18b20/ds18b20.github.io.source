---
title: TensorFlow-00-basic
date: 2018-05-12 22:57:20
categories: TensorFlow
tags:
- Basic
---

# 基本思想

TensorFlow基本思想是利用计算图来处理数据，把处理过程分成了两个独立的部分：定义计算图，执行计算图。官方表述为TensorFlow程序(tf.Graph)和TensorFlow运行时(tf.Session)。

当使用Estimator时，可以不涉及到具体的Graph和Session部分，这些都由Estimator在内部实现好了。

# 图(Graph)

计算图是一个由一系列TensorFlow命令组成的图，这些命令分成两种对象：

- 指令(op)：图的节点。描述张量如何被INPUT和OUTPUT。
- 张量：图的边线。表示流经图的数据值，大多数的TF函数都会返回tf.Tensor

## 指令(op)

见其他文章介绍

## 张量(tf.Tensor)

TF中的核心数据单位是张量(tf.Tensor)，张量就是一个基本数据类型的n维矩阵。张量的阶就是矩阵的维度，这也类似于Numpy中的np.ndarray。

tf.Tensor只是Graph的一个组成部分，不具有值，只是代表了流经图上边线的数据的属性。`tf.Tensor` 有以下属性：

- 数据类型（例如 `float32`，`int32` 或 `string`）
- 形状

特殊张量有以下几种：

- `tf.Variable`变量
- `tf.constant`常量
- `tf.placeholder`占位符
- `tf.SparseTensor`

除了`tf.Variable`，其他张量是不可变化的。

### 常量

```python
a = tf.constant([[3.0, 4.0]], dtype=tf.float32)
b = tf.constant(4.0)  # 默认也是tf.float32

total = a + b

print(a)
print(b)
print(total)
```

tf.constant()指令表示构建一个常量节点，这个节点没有输入值，而会把传递给构造函数的值(如上述代码中的[[3, 4]])输出。

代码执行结果，

```python
Tensor("Const:0", shape=(1, 2), dtype=float32)
Tensor("Const_1:0", shape=(), dtype=float32)
Tensor("add:0", shape=(1, 2), dtype=float32)
```

如上所述，Tensor不具有具体值，打印出的a/b/total并不是具体的值[[3, 4]]等，而是图中定义的**张量的特征**。

从打印结果可知，每个指令都有它的名称，这个名称不是由人定义，而是根据生成它所用的指令自动生成的。名称由`指令名`和`索引号码`组成，如上述`"Const:0"`前半部分Const表示由tf.contant指令生成的第0个节点。

常量包含以下几种：Constants, Sequences and Random Values，TF提供了生成它们的各种方法，参见https://www.tensorflow.org/api_guides/python/constant_op

### 占位符

占位符表示暂时不提供参数，但是承诺之后会提供。

```python
x = tf.placeholder(dtype=tf.float32, shape=(3, 3))
y = tf.matmul(x, x)
z = tf.multiply(x, x)

with tf.Session() as sess:
    print(sess.run(x, feed_dict={x: np.arange(9).reshape(3, 3)}))
    print(sess.run(y, feed_dict={x: np.arange(9).reshape(3, 3)}))
    print(sess.run(z, feed_dict={x: np.arange(9).reshape(3, 3)}))
```

可以使用Session的run方法里的feed_dict给占位符提供参数。不只是占位符，feed_dict参数可以给图中任意张量赋值，从而覆盖原来的张量。

注意，如果没有给占位符提供值，则会报错。

```
InvalidArgumentError (see above for traceback): You must feed a value for placeholder tensor 'Placeholder' with dtype float and shape [3,3]
```

而且，如果feed_dict的shape如果不匹配也会报错。

```
ValueError: Cannot feed value of shape (3, 4) for Tensor 'Placeholder:0', which has shape '(3, 3)'
```

### 变量

变量和其他Tensor的区别之一为Variable可变(mutable)，有assign方法，而其他Tensor是不可改变的。

详见https://blog.csdn.net/silent56_th/article/details/75577974

构建变量的最佳方式为tf.get_variable方法，此函数需要指定变量名称和形状。而具体的初始值则不必给出。

```python
my_variable = tf.get_variable("my_variable", [1, 2, 3], dtype=tf.int32, initializer=tf.zeros_initializer)
```

上述代码创建了一个名称为my_variable形状为[1,2,3]的变量，默认数据类型为tf.float32，默认初始化方法为glorot_uniform_initializer。关于tf.glorot_uniform_initializer可以参见https://stackoverflow.com/questions/37350131/what-is-the-default-variable-initializer-in-tensorflow

[From the documentation](https://www.tensorflow.org/api_docs/python/tf/get_variable):

> If initializer is `None` (the default), the default initializer passed in the variable scope will be used. If that one is `None` too, a `glorot_uniform_initializer` will be used.

The [`glorot_uniform_initializer`](https://github.com/tensorflow/tensorflow/blob/6bfbcf31dce9a59acfcad51d905894b082989012/tensorflow/python/ops/init_ops.py#L527) function initializes values from a uniform distribution.

也可以将 `tf.Variable` 初始化为 `tf.Tensor` 的值。这时不应指定形状，如

```python
other_variable = tf.get_variable("other_variable", dtype=tf.int32, initializer=tf.constant([23, 42]))
```

除了tf.get_variable方法，还可以使用tf.Variable方法，简单讲后者的实现更为底层。详见https://stackoverflow.com/questions/37098546/difference-between-variable-and-get-variable-in-tensorflow

而且，前者包含检查命名空间是否已存在相同变量名的功能。参见另一篇专门介绍variable的文章。

变量必须初始化之后才能使用，一次性初始化所有变量使用方法tf.global_variables_initializer() ，当然也可以单独对某个变量初始化。

```python
# all
session.run(tf.global_variables_initializer())
# single
session.run(my_variable.initializer)
# 查询未初始化变量
print(session.run(tf.report_uninitialized_variables()))
```

使用变量时，可以当作普通tf.Tensor来加入运算。为变量赋值，请使用 `assign`、`assign_add` 方法以及 `tf.Variable` 类中的友元。例如，以下就是调用这些方法的方式： 

```python
v = tf.get_variable("v", shape=(), initializer=tf.zeros_initializer())
assignment = v.assign_add(1)
tf.global_variables_initializer().run()
sess.run(assignment)  # or assignment.op.run(), or assignment.eval()
```

# 会话(Session)

要评估(查看)张量，需要实例化一个tf.Session对象。会话会封装TensorFlow运行时的状态，并运行TF指令。当使用tf.Session.run请求一个节点的输出张量时，TF会回溯整个计算图，并计算所有的作为请求节点输入的节点，将这些节点的输出传递给请求节点，运算并返回结果。

下面的命令创建一个tf.Session对象，然后运行命令run来评估total张量。

```python
sess = tf.Session()
print(sess.run(total))
```

可以同时传递多个张量给tf.Session.run

```python
with tf.Session() as sess:
    print(sess.run({'ab': (a, b), 'total': total}))
```

每当调用Session.run评估张量时，可以理解成输出此刻计算图的一个切面。比如下面的例子，每次调用run(vec)都会生成一组新的随机数，所以两次生成的随机数不同，但是后一次的结果用在+1和+2两次运算中，后一次的随机数列本身并没有发生变化。

```python
with tf.Session() as sess:
    vec = tf.random_uniform(shape=(3,))
    out1 = vec + 1
    out2 = vec + 2
    print(sess.run(vec))
    print(sess.run(vec))
    print(sess.run((out1, out2)))
```

输出结果，

```python
[0.35078466 0.6768017  0.9873116 ]
[0.7337662  0.9636233  0.79608345]
(array([1.8347821, 1.1241153, 1.2593273], dtype=float32), array([2.8347821, 2.1241155, 2.2593274], dtype=float32))
```

# Tensorboard

TF提供了一个将计算图可视化的工具Tensorboard，继续上方的代码，

```python
a = tf.constant([[3.0, 4.0]], dtype=tf.float32)
b = tf.constant(4.0)  # 默认也是tf.float32

total = a + b

print(a)
print(b)
print(total)

init = tf.global_variables_initializer()
with tf.Session() as sess:
    writer = tf.summary.FileWriter('./', sess.graph)
    writer.add_graph(tf.get_default_graph())
```

会在当前目录下生成`events.out.tfevents.*`文件，该目录下cmd执行`tensorboard --logdir .`按照输出提示在浏览器中打开指定地址即可查看到可视化界面。如下所示，

![](TensorFlow-00-basic\board.png)

# 参考

[使用低级别 TensorFlow API (TensorFlow Core) 开始编程](https://www.tensorflow.org/programmers_guide/low_level_intro)