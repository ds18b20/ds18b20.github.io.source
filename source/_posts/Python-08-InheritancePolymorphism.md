---
title: Python-08-InheritancePolymorphism
date: 2017-12-15 21:49:55
categories: Python
tags:
- Python
- class
- encapsulation
---

# Inheritance & Polymorphism in Python

When program in OOP, Python can create new classes from a present class. The new classes are called `Sub class`, and inherited class is called `Base class` or `Super class`.

## Inheritance

All member variables and methods are public by default. You can simply make a member public without any special ways. See codes below:

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
    
class Dog(Animal):
    pass
    
class Cat(Animal):
    pass

if __name__ == '__main__':
    Dog().run()
    Cat().run()
```

got:

```python
Animal is running...
Animal is running...
```



## Polymorphism

When sub classes have the same method/member with super classes, super classes method/member will be covered by sub classes method/member.

Like:

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
    	
class Dog(Animal):
    def run(self):
        print('Dog is running...')
    def swim(self):
        print('Dog is swimming...')
		
class Cat(Animal):
    def run(self):
        print('Cat is running...')
        
if __name__ == '__main__':
    Dog().run()
    Cat().run()
```

got:

```python
Dog is running...
Cat is running...
```

## Appendix

isinstance()

issubclass()