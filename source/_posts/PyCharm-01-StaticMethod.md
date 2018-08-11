---
title: PyCharm-01-StaticMethod
date: 2018-04-22 01:09:06
categories: PyCharm
tags:
- Python
---

# Method 'xxx' may be 'static

PyCharm "thinks" that you might have *wanted* to have a static method, but you forgot to declare it to be static.

PyCharm proposes this because the method does not *use* `self` in its body and hence does not actually *change the class instance*. Hence the method could be static, i.e. callable without having created a class instance before.

简单讲，就是函数中并没有引用到self的属性或者方法，这就跟class外面的函数作用相同。



# reference

https://stackoverflow.com/questions/23554872/why-does-pycharm-propose-to-change-method-to-static