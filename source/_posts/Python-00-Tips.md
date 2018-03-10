---
title: Python-00-Tips
date: 2018-02-21 21:36:01
categories: Python
tags:
- Python
- Tips
---

# a, b = b, a + b

`a, b = b, a + b`等价于

```python
t = (b, a + b)
a = t[0]
# 这里代入的a没有被更新
b = t[1]
```

而不是像C语言中，

```python
a = b
# a被上面一行更新了
b = a + b
```

这个技巧在Python中可以**用于交换两个变量**，

```python
a, b = b, a
```

仅仅一行就实现了C语言中三行的功能。

# in

判断一个元素是否存在于一个序列中，可以使用循环查询来实现。其实Python有个in的语法，可以更方便地达到目的。

```python
>>> a=range(10)
>>> 5 in a
True

>>> b = 'male'
>>> b in ['male', 'female']
True
```

