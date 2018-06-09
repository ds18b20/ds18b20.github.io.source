---
title: Python-34-sysargv
date: 2018-04-30 22:23:06
categories: Python
tags:
- sys
- argv
---

# sys.argv

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
import sys

def main(argv):
    if argv == None:
        print('Hello world')
    else:
        print(argv)

if __name__ == '__main__':
    main(sys.argv)
    print(type(sys.argv))
```

sys.argv是命令行下python命令后的所有参数的集合，这个集合组成了一个数组。

```
C:\Users\ds18b20\iPythonFiles>python arg.py -v
['arg.py', '-v']
<class 'list'>
```

