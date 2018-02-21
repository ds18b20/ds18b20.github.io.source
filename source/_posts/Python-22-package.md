---
title: Python-22-package
date: 2018-02-21 21:27:01
categories: Python
tags:
- Python
- package
---

# 模块的概念

Python中一个.py文件就是一个模块。例如，abc.py是名为abc的模块。

# 组织模块

通过包(package)来组织模块，来方便调用，避免模块名字的冲突。

```
myPackage
├─ __init__.py
├─ abc.py
└─ xyz.py
```

新建目录myPackage，并在其下方新建一个名为\_\_init\_\_.py的文件。此文件可以为空，有这个文件存在Python就会把这个目录当成一个包，否则只是当作普通的文件夹来处理。

\_\_init\_\_.py本身也是一个模块，它的模块名就是myPackage。

# 作用域

模块中的函数或变量加上前缀\_时，变成非公开的(private)，否则默认为公开的(public)。

```python
# 作用域

def _private_1(name):
    print('Hello, %s' % name)

def _private_2(name):
    print('Hi, %s' % name)
    
def public(name):
    if len(name) > 3:
        _private_1(name)
    else:
        _private_2(name)

public('Jim')
```

这样，外部不需要调用的函数全部定义成private，只有外部需要引用的函数才定义成public。

# 模块的导入

使用`import ModuleName`来导入。

模块的搜索顺序为：首先在当前目录下搜索，如果没有则去搜索所有已安装的内置模块和第三方模块。这些模块的路径保存在sys模块的path变量中，

```python
>>> import sys
>>> sys.path
['', 'C:\\ProgramData\\Anaconda3\\python36.zip',...]
```

如果要添加新的搜索路径，可以有两种方法：

一是，运行Python环境，修改sys.path。这种修改只在运行时有效，运行结束后失效。

```python
>>> import sys
>>> sys.path.append('/Users/michael/my_py_scripts')
```

二是，设置环境变量PYTHONPATH，运行结束不会失效。

参见文章https://www.cnblogs.com/qiyeshublog/archive/2012/01/24/2329162.html