---
title: Python-39-OpenEncoding
date: 2018-06-01 22:24:08
categories: Python
tags:
- open
- encoding
---

# open file with specified encoding

Python下直接用默认的open方法打开文件时，默认会使用系统默认的编码方式处理。如果包含默认编码方式无法打开的字符，会抛出错误：UnicodeDecodeError: 'gbk' codec can't decode byte...

```python
data = open(r'Text_data/1342-0.txt', 'r').read()
```

处理办法为，如果已知文件的编码方式，使用open方法时可以直接指定encoding参数。

```python
data = open(r'Text_data/1342-0.txt', 'r', encoding='UTF-8').read()
```

如果不知道文件使用的哪种编码方式，可以参考上一篇文章介绍的chardet库。

```python
import chardet

path=r"C:\Users\...\unicode.txt"
rawdata = open(path, 'rb').read()
result = chardet.detect(rawdata)
```

然后再指定encoding参数。



# Reference

https://www.cnblogs.com/arctique/p/5699620.html