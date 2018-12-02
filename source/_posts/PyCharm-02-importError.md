---
title: PyCharm-02-importError
date: 2018-08-07 23:22:31
categories: PyCharm
tags:
- import local

---

# 同目录下的py文件无法import

目录FFF下新建两个.py文件：a.py和b.py。

## 现象

在b.py内尝试`import a.py`，结果提示错误无法找到a.py。

## 原因

Pycharm不会将当前文件目录加入source_path内，所以需要手动加入。

## 解决方法

![](PyCharm-02-importError\import.png)

Pycharm的导航栏内右击目录-->Mark Directory as-->Source Root即可。