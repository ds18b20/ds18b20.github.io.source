---
title: Python-52-OSinfo
date: 2018-09-01 11:27:41
categories: Python
tags:
- os info
- Linux
- Windows

---

# Python查看当前OS

```python
import os
print(os.name)

import sys
print(sys.platform)

import platform
print(platform.system())

```

打印输出：

```python
'nt'
'Windows'
'Windows'
```

# Reference

https://blog.csdn.net/orangleliu/article/details/44907221

