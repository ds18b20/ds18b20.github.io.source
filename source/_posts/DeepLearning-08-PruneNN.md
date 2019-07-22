---
title: DeepLearning-08-PruneNN
date: 2019-03-01 23:15:31
categories: DeepLearning
tags:
- Deep Learning
- prune neural network

---

# prune fully connected layer

首先，初次训练2000次，得到一个有一定精度的模型。

![](DeepLearning-08-PruneNN\1st_train.png)

然后，使用threshold = 0.05来prune这个模型。具体是把所有`<threshold`的W参数强制改成0。得到一个sparse的模型：

![](DeepLearning-08-PruneNN\1st_train_prune.png)

之后，继续对该模型训练2000次，然后得到一个恢复一定程度的模型：

![](DeepLearning-08-PruneNN\2nd_train.png)

最后，可以同样对现在的模型prune一定的参数，同样使用threshold = 0.05，得到：

![](DeepLearning-08-PruneNN\2nd_train_prune.png)

