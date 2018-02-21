---
title: Python-24-slot
date: 2018-02-21 21:30:16
categories: Python
tags:
- Python
- __slot__
---

# 绑定属性和方法

Python是一种动态语言，允许在运行过程中给类和对象添加属性和方法。

## 针对实例

对于某一个实例做的修改只对当前实例有效，其他实例不受影响。

```python
# 一个空的类
class Student(object):
    pass
# 一个实例
s = Student()

# 添加attribute
s.name = 'Jim'
print(s.name)

# 添加method
# 注意需要引用Methodtype函数
from types import MethodType
def set_score(self, score):
    self.score = score
# 具体添加
s.set_score = MethodType(set_score, s)
s.set_score(95)
print(s.score)
```

输出，

```python
Jim
95
```

注意，

一是，新创建一个实例，并不会包含上述修改。

二是，给实例添加方法的时候，如果直接把新函数赋值给实例，会导致函数出错。把实参赋给self了

## 针对类

针对类做的修改会影响到所有新创建的实例。

```python
# 一个空的类
class Student(object):
    pass
# 一个实例
s = Student()
# 添加attribute
s.name = 'Jim'
print(s.name)

# 修改类的attribute
Student.name = 'Tom'
print(Student.name)
print(s.name)

# 添加method
def set_score(self, score):
    self.score = score

Student.set_score = set_score

s.set_score(95)
print(s.score)

s2 = Student()
print(s2.name)
```

输出

```python
Jim
Tom
Jim
95
Tom
```

可以看到，给类添加的方法对旧的实例仍然有效。但是新修改的类属性，并不能覆盖旧的实例的属性。

# \_\_slot\_\_

既然可以任意时刻添加属性到类中，有时候就需要给业已添加的属性加上限制。

```python
class Student(object):
  __slot__ = (name, age)
  pass

s = Student()
# 添加age没有问题
s.age = 25
# 添加score出错
# 因为不在__slot__内
s.score = 95
```

**值得注意的是**，继承的子类不受父类的slot设置。除非在子类中也加入了slot，这样子类允许定义的属性就是父类slot+子类slot的和集。