---
title: Python-33-argparse
date: 2018-04-30 20:15:01
categories: Python
tags:
- argparse

---

# Introduction

argparse是Python自带的一个命令行解析库，如字面意思所示可以方便的解析命令行下的参数。

# 用法

```python
#!/usr/bin/env python
# encoding: utf-8
import argparse

# 创建解析对象
parser = argparse.ArgumentParser()
# 向此对象中添加关注的命令行参数和选项
parser.add_argument() 
# 进行解析
parser.parse_args()

# 后续处理函数
```

参数主要分为位置参数(positional arguments)和可选参数(optional arguments)，其中位置参数是必要的参数，而可选参数通过`-参数名`来选择性的传递。

# positional arguments

位置参数不要求在前面加`-`，直接就可以使用。

```
import argparse

parse = argparse.ArgumentParser()
parse.add_argument("echo")
args = parse.parse_args()

print(args.echo)
```

上述代码的输出为，

```
C:\Users\ds18b20\iPythonFiles>python arg.py positional_argument
positional_argument
```

通过解析，将参数保存在args.echo之中，可以在后面继续调用。

上述代码如果不加位置参数会报错，

```
C:\Users\ds18b20\iPythonFiles>python arg.py
usage: arg.py [-h] echo
arg.py: error: the following arguments are required: echo
```

输入-h可以查看帮助信息，

```
C:\Users\ds18b20\iPythonFiles>python arg.py -h
usage: arg.py [-h] echo

positional arguments:
  echo

optional arguments:
  -h, --help  show this help message and exit
```

# optional arguments

```python
import argparse

parse = argparse.ArgumentParser()
parse.add_argument("-v", "--version", help="version to use")
args = parse.parse_args()

# print(args.v)
print(args.version)
```

传递可选参数时，像本例中需要加入-v或者--version提示，然后在后面跟上参数值。没有参数值会报错，而且传递的采纳数只会保存在args.version中，调用args.v会报错。

# action='store_true'

加上action限定之后，-v或--verbose之后并不需要接参数，而是直接像-h或--help那样实现一个布尔选择的功能，即True/False。

```python
#!/usr/bin/env python
# encoding: utf-8
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-v", "--verbose", help="increase output verbosity", action="store_true")
args = parser.parse_args()
print(args.verbose)
# if args.verbose:
        # print("verbosity turned on")
```

上例输出为True。

此外还有type(类型检查)，choices=\[\](备选值)等功能。。。



# Reference

python argparse用法总结

https://www.jianshu.com/p/fef2d215b91d