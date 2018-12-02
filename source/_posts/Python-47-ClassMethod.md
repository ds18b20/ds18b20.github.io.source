---
title: Python-47-ClassMethod
date: 2018-08-11 09:22:43
categories: Python
tags:
- class method
- classmethod
---

# Python ClassMethod

本篇介绍Python的类函数的定义，以及注意事项。

## @classmethod

类函数的定义方法为在函数上一行加上@classmethod装饰器。
并且类函数要求第一个参数不是self，而是cls。这里的cls代表当前类。

## 子类继承的注意事项

注意，当被子类继承时，cls不再是父类，而是子类。

```python
class Student(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def foo(cls):
        d = {'name': 'JJ', 'age': 30}
        return cls(**d)

    @classmethod
    def bar(cls):
        return cls.__name__
# s = Student('LL', 20)
# print(s.name)
# print(s.age)
# print(s.foo().name)
# print(s.foo().age)


class Boy(Student):
    def __init__(self):
        super(__class__, self).__init__('LL', 20)

b = Boy()
# print(b.foo().name)
print(dir(b))
print(b.bar())
```

b.bar()返回的是Boy，而非Student

# Reference

https://blog.csdn.net/carolzhang8406/article/details/6856817

> 由于classmethod第一个参数传入的是类对象，所以在子类继承后，使用子类调用该类方法，实际传入的对象将是子类对象，而不是父类对象。而staticmethod由于不需要传入类对象，所以它实际执行的还是父类当中的行为。