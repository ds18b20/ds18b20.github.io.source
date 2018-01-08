---
title: Python-04-dict
date: 2017-11-13 22:20:06
categories: Python
tags:
- Python
- dict

---

# Python Dict

## Create new dict

```python
a = {'name':'Tom', 'age':18}
b = dict(name='Tom', age='18')
c = dict([('name', 'Tom'), ('age', 18)])
```

or by `zip` method：

```python
keys = 'abcde'
values = range(1, 6)
zip_d = dict(zip(keys, values))
```

## iterate dict

```python
for a,b in zip_d.items():
    print(a,b)
```

## get/pop keys

You can use dict[key] / del dict[key] to get / delete a key. But if key does not exist,  Python will raise an exception that will crash your program.

So you could use:

```python
try: 
    value = my_dic[100]
except KeyError:
    print("key not found in dictionary")
```

or you could use get / pop method with default parameter.

```python
my_dic.get('key', 'get error value')
my_dic.pop('key', 'pop error value')
```

## combine 2 dicts

Three are 2 ways to combine 2 dicts:

```python
dict(dict1.items()+dict2.items())
# or
dict(dict1, **dict2)
```

We can use timeit to evaluate speed of these 2 ways.

```python
$ python -m timeit -s 'dict1=dict2=dict(a=1,b=2)' 'dict3=dict(dict1,**dict2)'

$ python -m timeit -s 'dict1=dict2=dict(a=1,b=2)' 'dict3=dict(dict1.items()+dict2.items())'
```

## dict.copy

If a dict / list contains another dict / list, be careful to use *.copy() .

As *.copy is shallow.

```
import copy

dict1 = {'a': [1, 2], 'b': 3}
dict2 = dict1
dict3 = dict1.copy()
dict4 = copy.deepcopy(dict1)

dict1['b'] = 'change'
dict1['a'].append('change')

print dict1  # {'a': [1, 2, 'change'], 'b': 'change'}
print dict2  # {'a': [1, 2, 'change'], 'b': 'change'}
print dict3  # {'a': [1, 2, 'change'], 'b': 3}
print dict4  # {'a': [1, 2], 'b': 3}
```

The same with a list:

```python
a=[[1,2],3,4]
b=a
c=a.copy()
import copy
d=copy.deepcopy(a)

a[2]=9 # [[1, 2], 3, 9]
>>> b
[[1, 2], 3, 9]
>>> c
[[1, 2], 3, 4]
>>> d
[[1, 2], 3, 4]

>>> a[0].append(8) # [[1, 2, 8], 3, 9]
>>> b
[[1, 2, 8], 3, 9]
>>> c
[[1, 2, 8], 3, 4]
>>> d
[[1, 2], 3, 4]
```

