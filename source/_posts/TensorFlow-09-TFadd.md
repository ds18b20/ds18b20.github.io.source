---
title: TensorFlow-09-TFadd
date: 2018-05-12 13:17:13
categories: TensorFlow
tags:
- add
- +
---

# tf.add和+的区别

tensorflow中定义运算图结构时，加法运算可以用tf.add表示，也可以直接使用加法运算符+表示。

从结果上讲，两种方式的实现结果相同。

```python
import numpy as np
import tensorflow as tf

x_data = np.random.rand(100).astype(np.float32)
y_data = 0.5 * x_data + 0.3

Weights = tf.Variable(tf.random_uniform([1], -1, 1))
biases = tf.Variable(tf.zeros([1]))

tf.reset_default_graph()
dtype = tf.int32
a = tf.placeholder(dtype)
b = tf.placeholder(dtype)
# c = a+b
c = tf.add(a, b)
print(tf.get_default_graph().as_graph_def())

print(dir(tf.Tensor))
```

返回结果，

```python
C:\Users\ds18b20\Anaconda3\envs\tensorflow\python.exe C:/Users/ds18b20/PycharmProjects/TensorFlow/tutorial/demo.py
node {
  name: "Placeholder"
  op: "Placeholder"
  attr {
    key: "dtype"
    value {
      type: DT_INT32
    }
  }
  attr {
    key: "shape"
    value {
      shape {
        unknown_rank: true
      }
    }
  }
}
node {
  name: "Placeholder_1"
  op: "Placeholder"
  attr {
    key: "dtype"
    value {
      type: DT_INT32
    }
  }
  attr {
    key: "shape"
    value {
      shape {
        unknown_rank: true
      }
    }
  }
}
node {
  # 注意这里
  name: "Add"
  op: "Add"
  input: "Placeholder"
  input: "Placeholder_1"
  attr {
    key: "T"
    value {
      type: DT_INT32
    }
  }
}
versions {
  producer: 26
}

['OVERLOADABLE_OPERATORS', '__abs__', '__add__', '__and__', '__array_priority__', '__bool__', '__class__', '__copy__', '__delattr__', '__dict__', '__dir__', '__div__', '__doc__', '__eq__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__invert__', '__iter__', '__le__', '__lt__', '__matmul__', '__mod__', '__module__', '__mul__', '__ne__', '__neg__', '__new__', '__nonzero__', '__or__', '__pow__', '__radd__', '__rand__', '__rdiv__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rmatmul__', '__rmod__', '__rmul__', '__ror__', '__rpow__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__weakref__', '__xor__', '_add_consumer', '_as_node_def_input', '_as_tf_output', '_c_api_shape', '_get_input_ops_without_shapes', '_override_operator', '_rank', '_shape', '_shape_as_list', '_shape_tuple', '_tf_api_names', 'consumers', 'device', 'dtype', 'eval', 'get_shape', 'graph', 'name', 'op', 'set_shape', 'shape', 'value_index']

Process finished with exit code 0
```

如果把`c = tf.add(a, b)`换成`c = a+b`，tensor量c的node类型名会变成"add"。

```python
...
node {
  name: "add"
  op: "Add"
  input: "Placeholder"
  input: "Placeholder_1"
  attr {
    key: "T"
    value {
      type: DT_INT32
    }
  }
}
...
```

参考链接1中的解释：

> There's no difference in precision between `a+b` and `tf.add(a, b)`. The former translates to `a.__add__(b)` which gets mapped to `tf.add` by means of [following line](https://github.com/tensorflow/tensorflow/blob/c43a32d5d0929170a057862e2cd0b59308421444/tensorflow/python/ops/math_ops.py#L845) in math_ops.py
>
> `_OverrideBinaryOperatorHelper(gen_math_ops.add, "add")`
>
> The only difference is that node name in the underlying Graph is `add` instead of `Add`. 

参考链接2中解释了，在x和y中至少有一个是tf.Tensor时，`x + y`才等价于`tf.add(x, y)`。这种情况下会生成一个TensorFlow op，而二者中的非Tensor量会被自动转换成tf.Tensor。若二者都不是tf.Tensor量，比如两个Numpy Array，那么不会生成TensorFlow op。

# 参考

[In tensorflow what is the difference between tf.add and operator (+)?](https://stackoverflow.com/questions/37900780/in-tensorflow-what-is-the-difference-between-tf-add-and-operator)

[TensorFlow operator overloading](https://stackoverflow.com/questions/35094899/tensorflow-operator-overloading)

[3.3.7. Emulating numeric types](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)

关于重载可以参见`Python-27-CustomizeClass`

