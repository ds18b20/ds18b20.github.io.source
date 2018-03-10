---
title: Python-00-FileClose
date: 2018-03-10 12:41:00
categories: Python
tags:
- file
- close
---



这篇文章主要测试一下文档的close方法。

# close方法的必要性

1. 操作系统的每个进程能打开的文件数目是有限的，当超过规定上限时会发生错误。
2. 另外，调用close方法才能把缓存中的数据同步到磁盘。

```python
import os

f = open('test.txt', 'w')
f.write('open test')
# 没有下面的关闭过程的话 调用remove会报错
# f.close()

os.remove('test.txt')
```

还有，在一个open过程中不能既写又读。

```python
f = open('test.txt', 'w')
f.write('open test')
# 此处错误
print(f.read())

f.close()
```

这样会抛出错误：`UnsupportedOperation: not readable`

```python
# 写过程
f = open('test.txt', 'w')
f.write('open test')
f.close()
# 读过程
f = open('test.txt', 'r')
print(f.read())
f.close()
```

读写分时就没有问题了。

# with

如果想书写更加方便，可以使用with方法省去写close过程。

```python
# 不需要close()
import os

with open('test.txt', 'w') as f:
    f.write('open test')

os.remove('test.txt')
```

这样remove就不会报错。