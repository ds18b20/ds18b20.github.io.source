---
title: Python-50-URL-XML-encoding
date: 2018-08-19 00:28:34
categories: Python
tags:
- URL
- XML
- HTML
- encode
- decode

---

# 编码

网络传输中经常能看到对信息的编码，编码的主要原因是传输的信息格式不符合协议要求（比如url中汉字的传输）还有需要加密的场合（对密码的传输）等。编码方式不公开的话编码就变成了加密过程。

下面记录一下URL的编码和HTML/XML的编码。

## URL的编码

对于URL的编码主要是为了适应传输协议要求，这是因为URL格式有如下规定，

> ...Only alphanumerics [0-9a-zA-Z], the special characters "$-_.+!*'()," [not including the quotes - ed], and reserved characters used for their reserved purposes may be used unencoded within a URL.

> 只有字母和数字[0-9a-zA-Z]、一些特殊符号"$-_.+!*'(),"[不包括双引号]、以及某些保留字，才可以不经过编码直接用于URL。

所以当URL地址中包含规定之外的字符时，需要编码成规定字符串，比如空格，中文字符。

对于中文字符常见以下4种情况：

1. 路径中包含汉字
   例如，Chrome地址栏内键入`Request URL: https://zh.wikipedia.org/wiki/编码`，然后通过开发者模式下查看request URl可得到`Request URL: https://zh.wikipedia.org/wiki/%E7%BC%96%E7%A0%81`。
   其中`/`后面跟的`%E7%BC%96%E7%A0%81`就是`编码`的utf-8编码后的结果。

在Python下键入`'编码'.encode()`得到`b'\xe7\xbc\x96\xe7\xa0\x81'`，可验证上述内容。

1. 参数中包含汉字和特殊字符

   1. *参数中包含汉字*

      例如，在Google中搜索关键字`编码`时，通过开发者模式查看network可得到request请求内容如下：

      `https://www.google.co.jp/search?newwindow=1&source=hp&ei=mApxW4jOFs-k-QbomZXwBw&q=%E7%BC%96%E7%A0%81&oq=&gs_l=psy-ab.1.0.35i39k1l6.0.0.0.14626.3.1.0.0.0.0.0.0..1.0....0...1c..64.psy-ab..2.1.219.6...220.gCjXlx2JHRc`
      `&q=`后面跟的`%E7%BC%96%E7%A0%81`也是转换成utf-8格式的`编码`。

   2. *参数中包含特殊字符*
      值得注意的是当参数重包含`&`和`=`时，也需要编码。
      这时因为参数是以`&key1=value1&key2=value2`的形式跟在path后面，如果value中包含`&`，比如`&key1=value&1&key2=value2`就会使得解码产生歧义。

2. GET/POST请求中包含汉字
   参见：http://www.ruanyifeng.com/blog/2010/02/url_encoding.html

3. Ajax调用的URL中包含汉字
   参见：http://www.ruanyifeng.com/blog/2010/02/url_encoding.html

### 编码/解码的实现

```python
from urllib import parse

# encode
path = 'http://www.example.org/'
keyword = u'编码'

encoded_keyword = parse.quote(keyword)
print('encoded keyword: {}'.format(encoded_keyword))

encoded_url = path + '?q=' + encoded_keyword
print('encoded url: {}'.format(encoded_url))

# 不希望编码的位置也被修改了，所以应该把URL的host和parameters分开编码
print(parse.quote('http://www.example.org/?q=编码'))

# decode
# 解码可以整个URL一起进行
decoded_url = parse.unquote(encoded_url)
print('decoded url: {}'.format(decoded_url))
```

打印输出：

```text
/anaconda3/bin/python3.6 /Users/huangqiying/PycharmProjects/Ablog/urlparse_test.py
encoded keyword: %E7%BC%96%E7%A0%81
encoded url: http://www.example.org/?q=%E7%BC%96%E7%A0%81
http%3A//www.example.org/%3Fq%3D%E7%BC%96%E7%A0%81
decoded url: http://www.example.org/?q=编码
```

> 关于url解析的详细内容可以参见另一篇文章：Python解析url

下面列举常见的转义字符：

[十六进制值]

1. \+  URL 中+号表示空格 %2B
2. 空格 URL中的空格可以用+号或者编码 %20
3. /  分隔目录和子目录 %2F 
4. ?  分隔实际的 URL 和参数%3F 
5. % 指定特殊字符 %25 
6. \# 表示书签 %23 
7. & URL 中指定的参数间的分隔符 %26 
8. = URL 中指定参数的值 %3D

当然可以在Python下简单验证如下：

```python
>>> parse.quote(' ')
'%20'
>>> parse.unquote('%20')
' '
```

## HTML/XML的编码

### XML

在XML的实体中不能出现`&`，`<`和`>`等特殊字符，否则编译会出错。比如，

```xml
<age> age < 30 </age> 
```

其中`<`会被理解成新元素的开始，而导致出错。相应地应该把这些特殊元素做编码处理：

```xml
<age> age &lt; 30 </age> 
```

转义的对应关系如下：

- &(逻辑与)-->\&amp;        
- <(小于)-->\&lt;        
- \>(大于)-->\&gt;        
- "(双引号)-->\&quot;      
- '(单引号)-->\&apos; 

对于转义，有如下要求：

a. 转义序列各字符间不能有空格；

b. 转义序列必须以"；"结束；

c. 单独的&不被认为是转义开始；

d. 区分大小写。

### HTML

由于HTML是XML的一个子集，对于转义字符除了XML的规定之外还有所增加。
具体参照：http://tool.oschina.net/commons?type=2

*转义字符：Escape Character*

常见的例子如下：

| HTML 原代码 | 显示 | 描述                   |
| ----------- | ---- | ---------------------- |
| `&lt;`      | <    | 小于号或显示标记       |
| `&gt;`      | >    | 大于号或显示标记       |
| `&amp;`     | &    | 可用于显示其它特殊字符 |
| `&quot;`    | “    | 引号                   |
| `&reg;`     | ®    | 已注册                 |
| `&copy;`    | ©    | 版权                   |
| `&trade;`   | ™    | 商标                   |
| `&ensp;`    |      | 半个空白位             |
| `&emsp;`    |      | 一个空白位             |
| `&nbsp;`    |      | 不断行的空白           |

## Html解码

html中转义后的字符串的解码可以使用html.unescape()函数实现。

```python
>>> html.escape("&<>")
'&amp;&lt;&gt;'
>>> html.unescape("&amp;&lt;&gt;")
'&<>'
```

# Reference

http://www.cnblogs.com/kaituorensheng/p/3927000.html
http://www.cnblogs.com/hyd309/p/3549076.html