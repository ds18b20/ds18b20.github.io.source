---
title: TensorFlow-07-ObjectDetection
date: 2018-05-04 18:43:28
categories: TensorFlow
tags:
- Object Detection

---

# TensorFlow Object Detection

Object Detection是TensorFlow自带的一个模型识别的API

# 安装

安装参考以下链接，

https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md

安装过程中遇到了`object_detection/protos/*.proto: No such file or directory`这个错误。

原因是使用了3.5版本的protoc，导致无法批量处理\*.proto文件。(\*代表通配符)

详见https://github.com/tensorflow/models/issues/2930

# from deployment import model_deploy ModuleNotFoundError: No module named 'deployment'

## 问题描述

执行trainer.py开始训练时，弹出此error，无法导入deployment模块。

```
python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config
```

## 原因

经调查可知deployment模块定义在了models/research/slim/目录下。Python执行时没有查找到此处，所以抛出了这个错误。

## 解决方法

1. 添加系统环境变量，在PYTHONPATH条目内添加`\tensorflow\models\research` and `\tensorflow\models\research\slim` 的完整路径。

2. cd到slim/目录下可以看到有setup.py文件，使用这个py文件可以安装slim模块到Anaconda的统一管理第三方模块的site-packages目录下。

   安装方法为，切换到slim/目录下，执行

   ```
   python setup.py build
   python setup.py install
   # 这时又抛出了一个错误，提示
   # error: could not create 'build': 当文件已存在时，无法创建该文件。
   # 解决方法见下文
   ```

注意，方法1在Linux环境下的命令为，

1. If [models](https://github.com/tensorflow/models) is located at /home/tensorflow/models/
2. Edit ~/.bashrc and puts
   `export PYTHONPATH="${PYTHONPATH}:/home/tensorflow/models:/home/tensorflow/models/slim/"`

经测，方法1在windows下无效，方法2可以完美解决问题。

安装之后，执行trainer.py，此错误不再抛出。

# error: could not create 'build': 当文件已存在时，无法创建该文件。

## 问题描述

执行`python setup.py install`安装slim模块时，抛出此错误。

## 原因

clone下来的代码库中自带了一个名为BUILD的文件，而install命令需要在当前文件夹下新建一个名为build的目录，此目录名和上述BUILD命名冲突，导致抛出此错误。

## 解决方法

把BUILD文件重命名为BUILD.bak备份，再执行`python setup.py install`。

## 参考

[Windows下Tensorflow-silm库使用遇到ImportError: No module named 'nets'问题的解决方法](https://blog.csdn.net/lgczym/article/details/79272579)

# TypeError: \`pred\` must be a Tensor, or a Python bool, or 1 or 0. Found instead: None

## 问题描述

执行trainer.py开始训练时，抛出此错误。

```
python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config、
```

## 原因

bug

## 解决方法

> FILE PATH: object_detection\models\
>
> change 109 line in ssd_mobilenet_v1_feature_extractor.py:
> from
>
> ```python
> is_training=None, regularize_depthwise=True)):
> ```
>
> to
>
> ```python
> is_training=True, regularize_depthwise=True)):
> ```

注意，目前ssd_mobilenet_v1_feature_extractor.py肯呢个存在于系统的多个目录下，Python调用的是已经安装到Anaconda下的site-packages目录下的.py。所以应该修改此处的.py文件。

# OOM when allocating tensor with shape\[\*, \*, ...\]

## 问题描述

执行trainer.py开始训练时，抛出此错误。

```
python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config、
```

## 原因

出现此错误提示，表示GPU内存不足(GPU ran out of memory)，此时可以手动减小batch_size的大小，默认设置为24。目前我用的机器GPU是GTX 960显存为4GB，batch_size减为10时，没有出现此错误。

## 内存消耗计算

\[1,144,144,144,128\] 矩阵大小为，1 * 144 * 144 * 128 = 382,205,952

如果数据类型是默认的32bit，则总占用显存为382,205,952 * 4 = 1 528 823,808

约为1.5G。所以仅仅tensorflow的计算就要消耗1.5G，还有其他显示消耗加到一起，不能超过显卡的物理内存大小。

## 解决方法

1. 增加显卡物理内存，根据计算量大小，把不同层计算分配到合适的GPU上。参见[[Is it possible to split a network across multiple GPUs in tensorflow?]](https://stackoverflow.com/questions/36313934/is-it-possible-to-split-a-network-across-multiple-gpus-in-tensorflow)

   ```python
   with tf.device("/gpu:0"):
     # Define first layer.
   
   with tf.device("/gpu:1"):
     # Define second layer.
   
   # Define other layers, etc.
   ```

2. 减小浮点数的精度(floating precision `tf.float64 -> tf.float32 -> tf.float16` )

3. 减小batch_size，ssd_mobilenet_v1_pets.config文件

   ```
   train_config: {
     # HERE
     batch_size: 10
     optimizer {
       rms_prop_optimizer: {
         learning_rate: {
           exponential_decay_learning_rate {
             initial_learning_rate: 0.004
             decay_steps: 800720
             decay_factor: 0.95
           }
         }
         momentum_optimizer_value: 0.9
         decay: 0.9
         epsilon: 1.0
       }
     }
     ...
     }
   ```

##参考

[OOM when allocating tensor with shape[1,144,144,144,128]](https://stackoverflow.com/questions/45704280/oom-when-allocating-tensor-with-shape1-144-144-144-128)

# Windows下查看显存

Win+R运行dxdiag打开`DirectX诊断工具`，切换到`显示`选项卡上即可看到显卡相关参数。