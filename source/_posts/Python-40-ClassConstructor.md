---
title: Python-40-ClassConstructor
date: 2018-06-21 23:09:39
categories: Python
tags:
- class
- constructor

---

# 类的构造函数

Python中新建一个类时，首先一般会在类下加入构造函数块`def __init__()`。

首先构造函数也是函数，在子类中可以被重写。和普通函数一样，重写之后的构造函数会把父类的构造函数覆盖。

如果在父类的构造函数中初始化了一些属性，而且在子类中没有显式地调用父类的构造函数，会导致在子类中无法找到父类属性的问题。

## 一个例子

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-


# 数据加载器基类
class Parent(object):
    def __init__(self):
        self.name = 'parent'

    def demo(self):
        print(self.name)
        print('OK')

class Child(Parent):
    def __init__(self):
        pass
        
    def demo(self):
        print(self.name)
        print('OK')


if __name__ == '__main__':
    a = Child()
    a.demo()
```

上述代码会抛出错误：

```
Traceback (most recent call last):
  File "class_test.py", line 36, in <module>
    a.demo()
  File "class_test.py", line 19, in demo
    print(self.name)
AttributeError: 'Child' object has no attribute 'name'
```

# 调用父类构造函数

调用父类构造函数有两种方式：

## 调用未绑定的超类构造方法

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-


# 数据加载器基类
class Parent(object):
    def __init__(self):
        self.name = 'parent'

    def demo(self):
        print(self.attr_p)
        print('OK')

class Child(Parent):
    def __init__(self):
        Parent.__init__(self)
        
    def demo(self):
        print(self.name)
        print('OK')


if __name__ == '__main__':
    a = Child()
    a.demo()
```

正常输出，

```
parent
OK
```

## 使用super函数

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-


# 数据加载器基类
class Parent(object):
    def __init__(self):
        self.name = 'parent'

    def demo(self):
        print(self.attr_p)
        print('OK')

class Child(Parent):
    def __init__(self):
        super(Child, self).__init__()
        
    def demo(self):
        print(self.name)
        print('OK')


if __name__ == '__main__':
    a = Child()
    a.demo()
```

输出，

```
parent
OK
```

# 注意

值得注意的是，如果子类中没有定义构造函数，则会自动继承父类的构造函数，这跟普通函数的继承一样。

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-


# 数据加载器基类
class Parent(object):
    def __init__(self):
        self.name = 'parent'

    def demo(self):
        print(self.attr_p)
        print('OK')

class Child(Parent):
    # def __init__(self):
        # super(Child, self).__init__()
        
    def demo(self):
        print(self.name)
        print('OK')


if __name__ == '__main__':
    a = Child()
    a.demo()
```

输出也是，

```
parent
OK
```

# 参考

[python中子类调用父类的初始化方法](https://www.cnblogs.com/nerrissa/articles/5607291.html)

[Python中子类初始化父类构造方法](https://www.jianshu.com/p/452e0fadd144)

