---
title: Python-49-parseURL
date: 2018-08-19 00:27:10
categories: Python
tags:
- URL
- urllib
- parse

---

# Python解析url

传入Server的url可以使用Python自带的urllib里的parse来解析。

## 解析query获取参数列表

假设有如下的一个URL字符串：

`http://www.example.org/default.html?ct=32&op=92&item=98#anchor`

下面的代码可以获取url内的参数：

```python
from urllib import parse
# Python3 replace `urlparse` with `urllib.parse`.

url = "http://www.example.org/default.html?ct=32&op=92&item=98#anchor"

# split url
split_result = parse.urlsplit(url=url)
print(split_result)

print('*********************************')
print('scheme: {}'.format(split_result.scheme))
print('netloc: {}'.format(split_result.netloc))
print('path: {}'.format(split_result.path))
print('query: {}'.format(split_result.query))
print('fragment: {}'.format(split_result.fragment))
print('*********************************')

# parse query string
query_string = split_result.query
print(parse.parse_qs(query_string))
```

解释如下：

### split url

- parse.urlsplit方法直接把url拆开成`scheme`，`netloc`，`path`，`query`，`fragment`5个部分。
  其中query就是以string形式保存的参数部分。
- fragment（片段）表示Html中的一个位置标记。例如`https://docs.python.org/3/library/urllib.parse.html#url-parsing`直接跳转到页面的`url-parsing`处。
  `#fragment`不会影响url查询结果，它只会作用于浏览器，告诉浏览器显示的位置。

### parse query string

parse.parse_qs(query_string)方法解析string格式的query语句，返回一个dict。

打印输出：

```text
SplitResult(scheme='http', netloc='www.example.org', path='/default.html', query='ct=32&op=92&item=98', fragment='anchor')
*********************************
scheme: http
netloc: www.example.org
path: /default.html
query: ct=32&op=92&item=98
fragment: anchor
*********************************
{'ct': ['32'], 'op': ['92'], 'item': ['98']}
```

### parse.urlparse()方法

parse模块下还有一个类似于urlsplit的方法，urlparse。https://docs.python.org/3/library/urllib.parse.html#urllib.parse.urlsplit
urlsplit可以大致作为为urlsplit的替代方法。二者唯一的区别就是urlparse会多得到一个params属性。
关于params参见：https://doriantaylor.com/policy/http-url-path-parameter-syntax
https://tools.ietf.org/html/rfc3986#section-3.3

## 解码汉字和%20

前一部分解析得到的url的query参数可能包含编码之后得到的字符串，只有通过解码才能得到真实的参数值。解码使用urllib.unquote()，下面是google搜索`编码`这个词的例子：

```python
from urllib import parse

url = "https://www.google.co.jp/search?newwindow=1&source=hp&ei=mApxW4jOFs-k-QbomZXwBw&q=%E7%BC%96%E7%A0%81&oq=&gs_l=psy-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..1.0....0...1c..64.psy-ab..2.1.219.6...220.gCjXlx2JHRc"
print(url)
q = parse.urlparse(url).query  # 获取query查询字符串
print('query:', q)
p = parse.unquote(q)  # 中文字符转换+%20替换
print('parsed query:', p)

qs = parse.parse_qs(q, True)
print('query dict:', qs)

```

打印输出：

```text
https://www.google.co.jp/search?newwindow=1&source=hp&ei=mApxW4jOFs-k-QbomZXwBw&
q=%E7%BC%96%E7%A0%81&oq=&gs_l=psy-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..
1.0....0...1c..64.psy-ab..2.1.219.6...220.gCjXlx2JHRc
query: newwindow=1&source=hp&ei=mApxW4jOFs-k-QbomZXwBw&q=%E7%BC%96%E7%A0%81&oq=&
gs_l=psy-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..1.0....0...1c..64.psy-ab.
.2.1.219.6...220.gCjXlx2JHRc
parsed query: newwindow=1&source=hp&ei=mApxW4jOFs-k-QbomZXwBw&q=编码&oq=&gs_l=ps
y-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..1.0....0...1c..64.psy-ab..2.1.21
9.6...220.gCjXlx2JHRc
query dict: {'gs_l': ['psy-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..1.0....
0...1c..64.psy-ab..2.1.219.6...220.gCjXlx2JHRc'], 'ei': ['mApxW4jOFs-k-QbomZXwBw
'], 'newwindow': ['1'], 'source': ['hp'], 'oq': [''], 'q': ['编码']}
```

# Reference

https://www.cnblogs.com/stemon/p/6602185.html