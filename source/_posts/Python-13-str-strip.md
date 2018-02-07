---
title: Python-13-str.strip
date: 2018-02-07 23:49:37
categories: Python
tags:
- Python
- string
- strip
---

#str.strip()

用于去除字符串头尾部指定的字符串，默认为空格。

基本语法为`str.strip([chars])`

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

str = "0000000     Runoob  0000000"; 
print str.strip( '0' );  # 去除首尾字符 0

str2 = "   Runoob      ";   # 去除首尾空格
print str2.strip();
```

只针对头尾部，中间的内容保留。