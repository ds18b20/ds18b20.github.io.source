---
title: Python-06-pickle
date: 2017-11-14 00:15:12
categories: Python
tags:
- Python
- pickle

---

# pickle

## serialization and deserialization

```python
#! /usr/bin/evn python
#-*- coding:utf-8 -*-

import pickle

def serialization(obj: object, path: str):
    with open(path, 'wb') as f:
        pickle.dump(obj, f)
        
def deserialization(path: str, obj: object):
    f_name=open(path,'rb')
    print('load result:',pickle.load(f_name))
    
if __name__ == '__main__': 
    path = 'test.txt'
    print('Path: {}'.format(path))
    obj = dict(name='xiao zhi',num=1002)
    print('Obj: {}'.format(obj))
    serialization(obj, path)
    deserialization(path, obj)
```

