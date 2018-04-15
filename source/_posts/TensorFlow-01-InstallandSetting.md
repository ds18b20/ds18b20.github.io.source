---
title: TensorFlow-01-InstallandSetting
date: 2018-04-08 11:00:46
categories: TensorFlow
tags:
- Install
- Setting
---

# CPU/GPU版本

TensorFlow有CPU和GPU两种安装类型，系统中没有安装NVIDIA的GPU时，只能安装使用CPU版本。但是，起步也可以使用CPU版本。

# Anaconda中安装

1. 首先确定已经已经安装Anaconda

2. 建立一个独立的conda开发环境，命名为tensorflow：

    

   ```
   C:\> conda create -n tensorflow 
   ```

3. 激活上述conda开发环境

   ```
   C:\> activate tensorflow
   (tensorflow)C:\>  # 你的提示符应该发生变化 
   ```

4. 安装

   安装CPU版本：

   ```
   (tensorflow)C:\> pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-1.1.0-cp35-cp35m-win_amd64.whl 
   ```

   安装支持 GPU 的 TensorFlow（写在一行）：

   ```
   (tensorflow)C:\> pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0-cp35-cp35m-win_amd64.whl 
   ```

# 参考

在 Windows 上安装 TensorFlow

https://efeiefei.gitbooks.io/tensorflow_documents_zh/install/install_windows.html