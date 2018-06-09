---
title: Python-36-PYTHONPATH
date: 2018-05-05 07:03:17
categories: Python
tags:
- PYTHONPATH
---

# Python的模块化编程

和其他编程语言相似，Python也支持模块化编程。一般一个程序文件就是一个模块，Python中就是一个个的.py文件，其他语言比如Java中的.java文件或者编译后的.class文件。在模块之上使用包来管理，简单来讲就是多个功能相关联的模块组成一个包。Java中把.class文件打包生成.jr作为包来供外部调用，而Python直接定义一个特殊的文件夹作为包来调用。

为了使Python能正确识别这些文件夹作为包，需要了解一下Python中对于包的规定。

## Python查找模块的路径

Python环境下调用sys.path可以查看当前Python的搜索路径。

```python
Python 3.5.4 |Anaconda, Inc.| (default, Nov  8 2017, 14:34:30) [MSC v.1900 64 bi
t (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', 'C:\\Users\\usr\\Anaconda3',...]
```

可以看到第一个路径是`''`，这意味着`相对路径下的当前路径`也在搜索路径之内。

如果当前路径下没有找到所需的模块，则会按列表顺序继续查找，直到找到目标模块。如果没有找到，则会抛出错误。

更改查找列表有两种方式：

1. 导入sys模块，通过sys.path.append(),sys.path.insert()等方法来修改。这种修改只适用于当前程序运行期间，重启之后修改失效。
2. 在系统环境变量中直接修改/新建PYTHONPATH，这种方式的设置永久有效。



# 参考

Python中的PYTHONPATH环境变量

http://www.cnblogs.com/geeklove01/p/7912435.html

Python的模块引用和查找路径

https://www.cnblogs.com/qingspace/p/5284480.html