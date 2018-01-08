---
title: Python-07-PublicProtectedPrivate
date: 2017-12-15 21:45:09
categories: Python
tags:
- Python
- class
- encapsulation
---

# Public Protected & Private in Python

Python doesn't have any mechanisms to restrict you from accessing a variable or calling a method belonging to an instance. But there are culture and convention that should be followed.

## Public

All member variables and methods are public by default. You can simply make a member public without any special ways. See codes below:

```python
class Cup(object):
    def __init__(self):
        self.color = None
        self.content = None
    def fill(self, beverage):
        self.content = beverage
    def empty(self):
        self.content = None

>>> redCup = Cup()
>>> redCup.color = 'red'
>>> redCup.content = 'coffee'
>>> redCup.content
'coffee'
```

## Protected

A protected member is accessible only from within the class and its subclasses. To make a member protected, you can prefix the name of your member with a single underscore. This means "This is a protected member. Don't touch it, unless you are a subclass."

Like:

```python
class Cup(object):
    def __init__(self):
        self.color = None
        self._content = None
    def fill(self, beverage):
        self._content = beverage
    def empty(self):
        self._content = None

redCup = Cup()
print(redCup._content)
redCup.color = 'red'
redCup._content = 'coffee'
>>> print(redCup._content)
'coffee'
```

Even prefixed with a single underscore, it is still OK to access this member. This method just tell some one not try to access this member.

## Private

As Python supports a technology called name mangling, you can make a member completely unaccessible by prefixing with at least 2 underscores and suffixing with at most 1 underscore. Python interpreter will change your member name to `_<className>__<memberName>`

Code:

```python
class Cup(object):
    def __init__(self):
        self.color = None
        self.__content = None
    def fill(self, beverage):
        self.__content = beverage
    def empty(self):
        self.__content = None

redCup = Cup()
print(redCup.__content)
```

Error:

```python
Traceback (most recent call last):
  File "v.py", line 13, in <module>
    print(redCup.__content)
AttributeError: 'Cup' object has no attribute '__content'
```

You can still access by `redCup._Cup__content` but DO NOT use this method.

## Appendix

isinstance()

issubclass()



beverage: drinking

encapsulation: 封装

prefix: 给...加前缀