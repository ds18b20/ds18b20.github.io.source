---
title: TensorFlow-05-compareCUPandGPU
date: 2018-05-03 23:43:33
categories: TensorFlow
tags:
- CPU
- GPU
---

# Compare speed with CPU/GPU

```python
# tensorflow_test.py

#!/usr/bin/env python
# -*- coding: utf-8 -*-
import sys
import numpy as np
import tensorflow as tf
from datetime import datetime

### argv[1] = type of device and which one
### argv[2] = size of the matrix to operate on

device_name = sys.argv[1]
shape = (int(sys.argv[2]), int(sys.argv[2]))
if device_name == "gpu":
    device_name = "/gpu:0"
else:
    device_name = "/cpu:0"

with tf.device(device_name):
    random_matrix = tf.random_uniform(shape=shape, minval=0, maxval=1)
    dot_operation = tf.matmul(random_matrix, tf.transpose(random_matrix))
    sum_operation = tf.reduce_sum(dot_operation)

startTime = datetime.now()
with tf.Session(config=tf.ConfigProto(log_device_placement=True)) as session:
        result = session.run(sum_operation)
        print(result)

### Print the shape, device name and timing
print("\n" * 3)
print("Shape:", shape, "Device:", device_name)
print("Time taken:", datetime.now() - startTime)
print("\n" * 3)

if __name__ == '__main__':
    pass
```

## run code with CPU

```
python tensorflow_test.py cpu 10000
```

got

> Shape: (10000, 10000) Device: /cpu:0
> Time taken: 0:00:11.039000

![](TensorFlow-05-compareCUPandGPU\CPU.JPG)

## run code with GPU

```
python tensorflow_test.py gpu 10000
```

got

> Shape: (10000, 10000) Device: /gpu:0
> Time taken: 0:00:01.880000

![](TensorFlow-05-compareCUPandGPU\GPU.JPG)

As this experiment shows, **GPU is about 6 times faster than CPU.**

# Reference

Test TensorFlow-GPU

http://bailiwick.io/2017/11/05/tensorflow-gpu-windows-and-jupyter/