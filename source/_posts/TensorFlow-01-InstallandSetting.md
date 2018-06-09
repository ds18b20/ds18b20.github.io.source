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

# Mac

Mac中的安装顺序：

使用 Virtualenv 进行安装 

1. 安装pip和virtualenv 

   ```
   $ sudo easy_install pip
   $ pip install --upgrade virtualenv
   ```

   

2. 创建一个virtualenv

   ```
   $ virtualenv --system-site-packages -p python3 targetDirectory # for Python 3.n
   ```

   

3. 激活virtualenv

   cd切换到上述targetDirectory目录，键入命令： 

   ```
   Air:~ hq$ cd ~/tensorflow
   Air:tensorflow hq$ source ./bin/activate (tensorflow)
   Air:tensorflow huangqiying$
   ```

   

4. 退出次virtualenv

   ```
   deactivate
   ```

## 编写bash文件

在Mac系统中可以编写bash script来实现类似Windows下的batch file的功能。

### 新建文件

文本文件内编写如下命令：

```
! /bin/bash

java -cp ".;./supportlibraries/Framework_Core.jar;./supportlibraries/Framework_DataTable.jar;./supportlibraries/Framework_Reporting.jar;./supportlibraries/Framework_Utilities.jar;./supportlibraries/poi-3.8-20120326.jar;PATH_TO_YOUR_SELENIUM_SERVER_FOLDER/selenium-server-standalone-2.19.0.jar" allocator.testTrack

```

保存为`scriptfilename.sh`

### 修改文件属性
然后通过下面的命令使得此文件可读可写可执行：
chmod 775 scriptfilename.sh

### 运行bash
最后cd切换到所在目录执行：
bash scriptfilename.sh

### 参考：
https://stackoverflow.com/questions/14065069/equivalent-of-bat-in-mac-os

# Training Data Set

## CIFAR10:

10 classes(airplane/automobile/bird...)

50,000 training images

10,000 testing images

32\*32 pixels

## MINIST

10 classes(1/2/3...)

50,000 training images

10,000 testing images

28\*28 pixels

# 参考

在 Windows 上安装 TensorFlow

https://efeiefei.gitbooks.io/tensorflow_documents_zh/install/install_windows.html