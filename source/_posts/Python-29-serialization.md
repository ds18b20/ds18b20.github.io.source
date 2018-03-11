---
title: Python-29-serialization
date: 2018-03-10 12:11:01
categories: Python
tags:
- Python
- serialization
---

# 序列化

在程序运行的过程中，所有的变量都保存在内存中。程序结束后，所有变量都被操作系统回收。

为了保存中间产生的数据，需要把内存中的变量转换成可以存储或者传输的数据。这个过程就叫做数据的序列化，即serialization，在Python中称为pickling。

序列化之后的数据可以存储到磁盘里，或者通过网络传输到其他机器上。反过来的过程叫反序列化，unpickling

# pickle

通过倒入pickle模块来实现。

## pickle.dumps(o)

把对象o序列化成一个bytes。

反序列化使用pickle.loads()

## pickle.dump(o, f)

把对象o序列化并保存到一个file-like-object中。

反序列化使用pickle.load()

```python
# use pickle
import pickle

with open('dump.txt', 'wb') as f:
    d = dict(name='Bob', age=28, score=95)
    pickle.dump(d, f)

with open('dump.txt', 'rb') as f:
    d = pickle.load(f)
    print(d)
```

注意，文件的打开方式为wb，因为pickle生成的是bytes类型的数据。

```python
with open('dump.txt', 'rb') as f:
    print(type(f))
# 得到<class '_io.BufferedReader'>

with open('dump.txt', 'r') as f:
    print(type(f))
# 得到<class '_io.TextIOWrapper'>
```

另外，通过python序列化后的bytes文件只能被python解析。而且不同版本的python可能也不兼容，故暂存重要数据时不要使用。

# JSON

若要在不同编程语言之间传递数据，需要把对象序列化为标准格式，例如XML和JSON。相比之下JSON更为流行，速度更快。

**JSON本身就是一个字符串。**

## json.dumps(o)

把对象o序列化成一个str。

反序列化使用pickle.loads()

```python
# use json
import json

d = dict(name='Bob', age=28, score=95)
# json.dumps(d)
```

## json.dump(o, f)

把对象o序列化并保存到一个file-like-object中。

反序列化使用pickle.load()

```python
# use json
import json

# 此处最好指定编码类型
with open('dump.txt', 'w', encoding='utf8') as f:
    json.dump(d, f)
# 此处最好指定编码类型
with open('dump.txt', 'r', encoding='utf8') as f:
    print(json.load(f))
```

JSON标准规定JSON编码是UTF-8

顺便说一下，对于编码类型的名称，uft-8/utf8/utf8都可以。

## 序列化对象

通过上述例子，可以知道dict可以简单地序列化为一个str。但是对于自己创建的类生成的实例，json模块并不知道怎样处理。这时需要指定转换的方法，

```python
# use json
import json

class Student(object):
    def __init__(self, name, age, score):
        self.__name = name
        self.__age = age
        self.__score = score
    def foo(self):
        pass

def student2dict(std):
    return {
        'name': std._Student__name,
        'age': std._Student__age,
        'score': std._Student__score,
    }

def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

# 序列化
s = Student('Bob', 28, 96)
with open('dump.txt', 'w', encoding='utf8') as f:
    # 或者用lambda表达式
    json.dump(s, f,default=student2dict)

# 反序列化
with open('dump.txt', 'r', encoding='utf8') as f:
    # 得到的是dict对象
    print(json.load(f))
    
with open('dump.txt', 'r', encoding='utf8') as f:
    print(json.load(f, object_hook=dict2student))
```

\#处可以使用lambda表达式简写为，

```python
json.dump(s, f,default=lambda obj: obj.__dict__)
```

因为除了定义了\_\_slot\_\_之外的类的实例都包含\_\_dict\_\_属性。

Likewise，load函数也可以添加object_hook函数来生成Student实例。

# 参考

Json module

https://docs.python.jp/3/library/json.html#module-json

Tutorial

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143192607210600a668b5112e4a979dd20e4661cc9c97000