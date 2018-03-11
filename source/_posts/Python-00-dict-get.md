---
title: Python-00-dict-get
date: 2018-03-10 12:27:29
categories: Python
tags:
- dict
- get
---



这里记录一下，dict获取键值的两种方法，dict['key']和dict.get('key')

# dict['key']

有key键存在时，返回键的值；

key键不存在时，返回KeyError

```python
dataDict = {'a': 23, 'b':89}
# KeyError
dataDict['c']
```

# dict.get('key')

有key键存在时，返回键的值；

key键不存在时，返回None(默认)

```python
dataDict = {'a': 23, 'b':89}
# 返回23
dataDict.get('a')
# 返回None
dataDict.get('c')
```

完整用法为，

```python
dict.get(key, default=None)
```

- key -- 字典中要查找的键。
- default -- 如果指定键的值不存在时，返回该默认值。

故可以通过dict.get()方法检验键是否存在，而且不会抛出Error。