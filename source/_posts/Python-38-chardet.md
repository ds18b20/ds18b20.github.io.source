---
title: Python-38-chardet
date: 2018-05-20 10:22:17
categories: Python
tags:
- charset

---

# 编码方式检测

使用charset库的detect方法检测网页的编码方式。

以下代码是检测本地的文本文件的编码方式的一个例子：

```python
>>> import chardet

>>> path=r"C:\Users\...\unicode.txt"
>>> rawdata = open(path, 'rb').read()
>>> result = chardet.detect(rawdata)
>>> result
{'encoding': 'GB2312', 'confidence': 0.99, 'language': 'Chinese'}
```

# 参考

[Character detection in a text file in Python using the Universal Encoding Detector (chardet)](https://stackoverflow.com/questions/3323770/character-detection-in-a-text-file-in-python-using-the-universal-encoding-detect)

http://zoeyyoung.github.io/python-chardet.html