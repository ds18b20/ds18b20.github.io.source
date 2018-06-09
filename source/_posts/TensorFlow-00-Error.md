---
title: TensorFlow-00-Error
date: 2018-05-02 23:32:18
categories: TensorFlow
tags:
- Error
---

# Error-Use GPU

在GPU上运行tensorflow代码时，出现了如下错误提示

> ImportError: Could not find 'cudart64_90.dll'. TensorFlow requires that this DLL be installed in a directory that is named in your %PATH% environment variable.
> Download and install CUDA 9.0 from this URL: https://developer.nvidia.com/cuda-toolkit

原因是安装的CUDA版本太新（9.1），和tensorflow不匹配，可以通过安装旧版（9.0）来解决这个问题。对应的cuDNN库克需要退回到相应版本。

## 参考

ImportError: Could not find 'cudart64_90.dll'.

https://github.com/tensorflow/tensorflow/issues/17101



# Nan in summary histogram for:***

这个Error常见于hyper parameter设置不合理导致的梯度消失或者梯度爆炸上，所以可以调整网络结构(W的个数等)或者学习速率来消除。

> Usually NaN is a sign of model instability, for example, exploding gradients. It may be unnoticed, loss would just stop shrinking. Trying to log weights summary makes the problem explicit. I suggest you to reduce the learning rate as a first measure. 

参考：https://stackoverflow.com/questions/39854390/nan-in-summary-histogram

>  The error comes through the learning rate, which is to high for this data set. If you set the learning rate to 0.001 you won't have this error anymore and can finetune on the cats vs dogs dataset without problems. 

参考：https://github.com/kratzert/finetune_alexnet_with_tensorflow/issues/23