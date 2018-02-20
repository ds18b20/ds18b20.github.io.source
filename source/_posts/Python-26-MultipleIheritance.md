---
title: Python-26-MultipleIheritance
date: 2018-02-20 23:39:49
categories: Python
tags:
- Python
- Multiple Inheritance
---

# python多重继承

如果对象可以用多种方式分组，当把这些分组统一在一个体系下标识时，分组个数会呈指数级增长。这给组织逻辑带来很大挑战，而且不便于理解。

可以使用多重继承(Multiple Inheritance)来代替复杂的单一继承关系。

```python
class Animal(object):
    pass

# 主线大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 功能类
class RunnableMixIn(object):
    def run(self):
        print('Running...')

class FlyableMixIn(object):
    def fly(self):
        print('Flying...')

# 多重继承
class Dog(Mammal, RunnableMixIn):
    pass

h = dog()
h.run()
```

# 具体应用原则

设计多重继承关系时，可以采用如下原则：

主线继承采用单一继承，例如Mammal和Bird单一继承自Animal。

而如果需要给对象搭配“混入”一些功能，则可以加入多重继承。比如，上面例子中给Dog添加Run的功能。习惯上为了便于识别，命名时在最后加上MixIn。