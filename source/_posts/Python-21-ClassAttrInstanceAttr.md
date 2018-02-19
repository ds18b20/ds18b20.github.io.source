---
title: Python-21-ClassAttrInstanceAttr
date: 2018-02-19 22:45:53
categories: Python
tags:
- Python
- Class Attribute
- Instance Attribute
---

# 类属性

```python
# 定义
class Student(object):
    name = 'Student'

# 外部引用
Student.name

# 内部引用
class Student(object):
    name = 'Student'
    def innerFunc(self):
        print(Student.name)

# 修改
Student.name = 'abc'
```

类的属性，无论是内部引用还是外部引用，都要使用`Class.attribute`的方式。

# 实例属性

```python
# 定义
class Student(object):
    def __init__(self, name)
        self.name = 'Student'
s = Student('Tom')

# 外部引用
s.name

# 内部引用
class Student(object):
    def __init__(self, name)
        self.name = 'Student'
        
    def foo(self):
        print(self.name)
s = Student('Tom')

# 修改
# 只是演示，不推荐这样
s.name = 'abc'
```

实例的属性，内部引用使用`self.attribute`，外部引用时使用`instance.attribute`的方式。

# 实例

实现每创建一个实例，计数加一的功能：

```python
class Student(object):
    count = 0
    def __init__(self, name):
        self.name = name
        # 此处就是引用了Class的属性
        Student.count += 1
        
# 测试:
if Student.count != 0:
    print('测试失败!')
else:
    bart = Student('Bart')
    if Student.count != 1:
        print('测试失败!')
    else:
        lisa = Student('Bart')
        if Student.count != 2:
            print('测试失败!')
        else:
            print('Students:', Student.count)
            print('测试通过!')
```

由于count是Class属性，故引用时需要Student.count的方式。

