---
title: Python-51-getFunctionNameClassName
date: 2018-09-01 10:58:36
categories: Python
tags:
- function name
- class name
- line number

---

# get function name

代码示例：

```python
import sys
import inspect
import os


def get_current_function_name():
    return inspect.stack()[1][3]


def get_attrs():
    print('Module:', __name__)
    print('File path: ', __file__)
    print('File name: ', os.path.basename(__file__))
    print('Current line No.: ', sys._getframe().f_lineno)
    print('Func name: ', sys._getframe().f_code.co_name)
    print('Func name: ', get_current_function_name())
    
if __name__ == '__main__':
    # get function name
    get_attrs()
```

打印输出：

```
Module: __main__
File path:  C:/Users/ds18b20/PycharmProjects/DeepLearning/test/test.py
File name:  test.py
Current line No.:  16
Func name:  get_attrs
Func name:  get_attrs
```

Python以文件的方式组织模块，每个.py文件都是一个最基本的Python模块。当模块直接被执行时，模块内的\_\_name\_\_属性被置为\_\_main\_\_，而当模块被import时，\_\_name\_\_属性就是模块的文件名。

# get class name

代码示例：

```python
class Foo(object):
    def __init__(self):
        self.a = "xxx"
        self.b = "yyy"
        
if __name__ == '__main__':
    f = Foo()
    print(f.__class__.__name__)
    print(f.__dict__)
    print('{a}, {b}'.format(**f.__dict__))
```

打印输出：

```
Foo
{'a': 'xxx', 'b': 'yyy'}
xxx, yyy
```

# Reference

https://blog.csdn.net/yongche_shi/article/details/50554015

https://blog.csdn.net/meta_cpp/article/details/80506588

