---
title: Python-42-immutable
date: 2018-06-24 04:32:55
categories: Python
tags:
- immutable

---

# 不可变变量

# error

```python
>>> s='123'
>>> s[0]='a'
```

当尝试对string的元素进行修改时，抛出错误：

```
TypeError: 'str' object does not support item assignment
```

这是因为string类型和Int，float一样是immutable，即不可变变量。

相对应地，list属于可变变量。