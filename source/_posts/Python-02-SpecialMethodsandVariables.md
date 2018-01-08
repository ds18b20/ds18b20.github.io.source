---
title: Python-02-SpecialMethodsandVariables
date: 2017-10-10 22:24:15
categories: Python
tags:
- Python
---

# Special Methods and Variables in Python

## \__name__

```
if __name__ == "__main__":
	print("Hello")
```

## \__doc__

\__doc__用来访问模块，类声明或者函数的声明中第一个未被赋值的字符串。字符串可以是单引号/双引号/三引号括起来的部分。

要访问\_\_doc__，可以使用

```python
obj.__doc__
```

其中obj是模块/类/函数的名称。

使用这个语法可以方便的查看函数的简介。

[Example](http://exampleprogramming.com/specialvars.html)