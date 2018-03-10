---
title: DataSerializationFormat-XML
date: 2018-03-10 12:50:04
categories: Data
tags:
- Serialization
- XML
---



html xhtml xml比较

- html(Hyper Text Markup Language)
- xhtml(eXtensible Hyper Text Markup Language)
- xml(eXtensible Markup Language)

这三种标记语言都是SGML(Standard Generalized Markup Language)的子集，诞生时间和语法规范有不同。总体来讲，时间上是html->xhtml->xml的顺序，语法规范的严格度上讲，xml>xhtml>html。

下面重点讲解xml和html的区别。

# xml

eXtensible Markup Language按字面理解：

1. 可扩展。可以用作任何的场合：配置文件，UI描述文件等。
2. 是一种标记语言。用来组织文本数据，结构清晰易读。

应用之一，代替.ini文件写配置文件。方便直观，容易被程序解析（正反序列化）。

`既然有了XML那为什么还用JSON呢？`

xml功能强大，语法严谨。但是，标记数据时产生很多冗余，这些冗余在网络上进行数据时，浪费大量网络资源。反过来讲，正是这些荣誉信息的存在，xml才更加对人友好。

## 语法

```xml
< ?xml version="1.0" encoding="UTF-8"? >
< ?xml-stylesheet href="style.css" type="text/css"? >

<student1>
  <name>Tom</name>
  <math>95</math>
  <biology>93</biology>
</student1>
<student2>
  <name>John</name>
  <math>95</math>
  <biology>93</biology>
</student2>
```

一个标准的xml文件包含以下部分：

### xml文件宣言

> < ?xml version="1.0" encoding="UTF-8"? >

说明这是一个xml文件，以及版本号编码方式。

### 样式设定

> < ?xml-stylesheet href="style.css" type="text/css"? >

用来标示xml的格式化显示的设定。如果要使用xsl样式，需要这样写：

> < ? xml-stylesheet href="style.xsl" type="text/xsl" ? >

### 元素和属性

> <student1>
>   <name>Tom</name>
>   <math>95</math>
>   <biology>93</biology>
> </student1>

<**>称为标签(tags)，可以包含元素(element)和属性(attributes)两部分。

起始标签：`<元素名 属性="属性值">`注意要有引号！

结束标签：`</元素名>`

上例中的最高层元素student1称为根元素(root element)

# 和html的区别

1. xml标签区分大小写，html不区分。
2. xml要求起始标签和结束标签必须成对存在，html有时候缺少结束标签也可以解析。
3. xml空元素需要在右侧尖括号前加上斜杠(如\<example/>)。html中的\<br>\<img>这类标签自成一个单元，在xml中成为空元素。
4. xml把空白当成内容的一部分。html则会过滤掉大部分空白。

# 参考

XML入門

http://yes.nctu.edu.tw/Lecture/NewTech/C05_NewEra/XML/IntroXML/

