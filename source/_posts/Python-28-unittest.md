---
title: Python-28-unittest
date: 2018-02-25 13:00:20
categories: Python
tags:
- Python
- unittest
---

# 单元测试

编写单元测试模块，对自己的代码进行测试。

## 对象模块

例如，先自己写一个模块。具体实现功能为，行哦那个dict类继承得到一个新类Dict，并给它添加一个访问属性的功能。保存为mydict.py

```python
# 添加访问属性的功能
class Dict(dict):
    # 关键字参数
    def __init__(self, **kw):
        # 传入一个字典kw
        super(Dict, self).__init__(**kw)
        
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError('Dict has no attribute: %s' % key)
    
    def __setattr__(self, key, value):
        self[key] = value
```

## 测试模块

接下来编写测试模块，保存为mydict_test.py

```python
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):
    
    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))
        
    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')
        
    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')

    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty
            
if __name__ == '__main__':
    unittest.main()
```

直接在mydict_dict.py目录下运行此Py文件。

测试模块中的测试方法，需要以test开头。不然与啊你先那个测试模块时不会自动运行。

## 运行测试

```python
# 命令行下
python mydict_test.py
```

