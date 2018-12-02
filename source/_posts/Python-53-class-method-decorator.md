---
title: Python-53-class-method-decorator
date: 2018-09-01 12:38:29
categories: Python
tags:
- class method decorator

---

# class method decorator

Python 装饰器装饰类中的方法

decorator中调用被装饰的函数时，在调用函数的参数列表最前面加入self参数的话，下文中就可以直接调用class中的方法self.func()了。

```python
def catch_exception(origin_func):
    def wrapper(self, *args, **kwargs):
        try:
            u = origin_func(self, *args, **kwargs)
            return u
        except Exception:
            self.revive() #不用顾虑，直接调用原来的类的方法
            return 'an Exception raised.'
    return wrapper
class Test(object):
    def __init__(self):
        pass
    def revive(self):
        print('revive from exception.')
        # do something to restore
    @catch_exception
    def read_value(self):
        print('here I will do something.')
        # do something.
```



# Reference:

https://kingname.info/2017/04/17/decorate-for-method/