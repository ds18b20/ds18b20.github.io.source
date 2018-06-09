---
title: TensorFlow-04-deviceCPUorGPU
date: 2018-05-02 21:52:27
categories: TensorFlow
tags:
- CPU
- GPU
---

# CPU or GPU?

若要判断CPU和GPU是否可用，可以使用以下方法：

## 方法1

To find out which device is used, you can enable log device placement like this:

```
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
```

## 方法2

Apart from using `sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))` which is outlined in other answers as well as in TF official [documentation](https://www.tensorflow.org/tutorials/using_gpu), you can try to assign a computation to the gpu and see whether you have an error.

```
import tensorflow as tf
with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print (sess.run(c))
```

Here

- "/cpu:0": The CPU of your machine.
- "/gpu:0": The GPU of your machine, if you have one.

If you have a gpu and can use it, you will see the result. Otherwise you will see an error with a long stacktrace. In the end you will have something like this:

> Cannot assign a device to node 'MatMul': Could not satisfy explicit device specification '/device:GPU:0' because no devices matching that specification are registered in this process

实测结果如下，

```
(tensorflow) C:\Users\username\PycharmProjects\DeepGTA5>python GTA5_11_tensorflow
_test.py
2018-05-02 21:42:08.599200: I T:\src\github\tensorflow\tensorflow\core\platform\
cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow bi
nary was not compiled to use: AVX2
Traceback (most recent call last):
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1327, in _do_call
    return fn(*args)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1310, in _run_fn
    self._extend_graph()
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1358, in _extend_graph
    graph_def.SerializeToString(), status)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\framework\errors_impl.py", line 516, in __exit__
    c_api.TF_GetCode(self.status.status))
tensorflow.python.framework.errors_impl.InvalidArgumentError: Cannot assign a de
vice for operation 'MatMul': Operation was explicitly assigned to /device:GPU:0
but available devices are [ /job:localhost/replica:0/task:0/device:CPU:0 ]. Make
 sure the device specification refers to a valid device.
         [[Node: MatMul = MatMul[T=DT_FLOAT, transpose_a=false, transpose_b=fals
e, _device="/device:GPU:0"](a, b)]]

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "GTA5_11_tensorflow_test.py", line 11, in <module>
    print (sess.run(c))
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 905, in run
    run_metadata_ptr)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1140, in _run
    feed_dict_tensor, options, run_metadata)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1321, in _do_run
    run_metadata)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\client\session.py", line 1340, in _do_call
    raise type(e)(node_def, op, message)
tensorflow.python.framework.errors_impl.InvalidArgumentError: Cannot assign a de
vice for operation 'MatMul': Operation was explicitly assigned to /device:GPU:0
but available devices are [ /job:localhost/replica:0/task:0/device:CPU:0 ]. Make
 sure the device specification refers to a valid device.
         [[Node: MatMul = MatMul[T=DT_FLOAT, transpose_a=false, transpose_b=fals
e, _device="/device:GPU:0"](a, b)]]

Caused by op 'MatMul', defined at:
  File "GTA5_11_tensorflow_test.py", line 8, in <module>
    c = tf.matmul(a, b)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\ops\math_ops.py", line 2108, in matmul
    a, b, transpose_a=transpose_a, transpose_b=transpose_b, name=name)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\ops\gen_math_ops.py", line 4492, in mat_mul
    name=name)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\framework\op_def_library.py", line 787, in _apply_op_helper
    op_def=op_def)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\framework\ops.py", line 3290, in create_op
    op_def=op_def)
  File "C:\Users\ds18b20\Anaconda3\envs\tensorflow\lib\site-packages\tensorflow\
python\framework\ops.py", line 1654, in __init__
    self._traceback = self._graph._extract_stack()  # pylint: disable=protected-
access

InvalidArgumentError (see above for traceback): Cannot assign a device for operation 'MatMul': Operation was explicitly assigned to /device:GPU:0 but available
devices are [ /job:localhost/replica:0/task:0/device:CPU:0 ]. Make sure the device specification refers to a valid device.
         [[Node: MatMul = MatMul[T=DT_FLOAT, transpose_a=false, transpose_b=fals
e, _device="/device:GPU:0"](a, b)]]
```

stacktrace提示CPU:0可用，但是代码中指定的是GPU:0，而GPU不可用。

> InvalidArgumentError (see above for traceback): Cannot assign a device for operation 'MatMul': Operation was explicitly assigned to /device:GPU:0 but available devices are [ /job:localhost/replica:0/task:0/device:CPU:0 ]. Make sure the device specification refers to a valid device.

# 参考

How to tell if tensorflow is using gpu acceleration from inside python shell?

https://stackoverflow.com/questions/38009682/how-to-tell-if-tensorflow-is-using-gpu-acceleration-from-inside-python-shell