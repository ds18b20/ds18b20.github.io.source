---
title: Crawler-02-FindElement
date: 2018-03-10 12:21:11
categories: Crawler
tags:
- Selenium
- Install
---



本章节主要介绍通过id和XPath定位元素的方法。

由于id具有唯一性，有id的情况下通过id能最快速精准地得到目标元素。

但是，并不是所有元素都有唯一的id标示。这时可以使用XPath借助属性定位。

另外，也顺带说一下其他几种定位方法。

# prepare html

操作对象为www.baidu.com

```html
<!--- 输入框部分 --->
<span class="bg s_ipt_wr quickdelete-wrap"><span class="soutu-btn"></span>
<input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
<a href="javascript:;" id="quickdelete" title="清空" class="quickdelete" style="top: 0px; right: 0px; display: none;"></a>
</span>

<!--- 搜索按钮部分 --->
<span class="bg s_btn_wr"><input type="submit" id="su" value="百度一下" class="bg s_btn"></span>
```

分析上述html编码可知，输入框部分input标签的`id: kw`，按钮部分input的`id: su`。

# find by id

在定位元素之前，需要获取Web文件

```python
from selenium import webdriver

driver = webdriver.Chrome('C:\Windows\System32\chromedriver')
#open the Homepage
driver.get("http://www.baidu.com")
```

然后，定位到输入框，并传入搜索关键字selenium

```python
# Find element -> pass keyword
try:
    driver.find_element_by_xpath("//input[@id='kw']").send_keys('selenium')
    print('find by id success!')
except:
    print('find by id error!')
```

之后，定位到搜索按钮，执行搜索

```python
#click to search
try:  
    driver.find_element_by_xpath("//input[@id='su']").click()  
    print('click success!')  
except:  
    print('click error!') 
```

可以看到，chrome启动，打开www.baidu.com的网页，并且输入关键字，显示出了搜索结果。

## full code

完整版code如下，

```python
# coding=utf-8

from selenium import webdriver

driver = webdriver.Chrome('C:\Windows\System32\chromedriver')
#open the Homepage
driver.get("http://www.baidu.com")

# Find element -> pass keyword
try:
    driver.find_element_by_id('kw').send_keys('selenium')
    print('find by id success!')
except:
    print('find by id error!')

#click to search
try:  
    driver.find_element_by_id('su').click() 
    print('click success!')  
except:  
    print('click error!')
# 可以用.quit()关闭窗口
driver.quit()
```



# find by XPath

XPath是XML文档中定位元素的语言，selenium中也有类似功能的实现。需要特别注意的是，当同级别存在多个标签时，索引从1开始，并非从0开始。

## 绝对路径定位

语法格式为，xxx.find_element_by_xpath("绝对路径")

例如， xxx.find_element_by_xpath("/html/body/div[x]/form/input") 

其中x 代表第x个 div标签，注意索引从1开始而不是0

此方法最为直接了当，但是随着网页的更新，需要定位的元素位置很可能发生改变，这就给代码的通用性带来很大问题。

## 相对路径定位

相对路径以//开始，后面跟着标签名。其语法格式为，

 xxx.find_element_by_xpath("//标签")

例如，

xxx.find_element_by_xpath("//input[x]") 

定位到第x个input标签,[x]可以省略，**默认为第一个**

也可以增加路径长度，xxx.find_element_by_xpath("//div[x]/form[x]/input[x]")

## 标签属性定位

给标签加入限定的属性条件，来实现更精准的查找。如果结果存在多个，默认为第一个。

xxx.find_element_by_xpath("//标签[@属性==‘属性值’]")

结合`相对路径定位`可以有以下的方法，

xxx.find_element_by_xpath("//标签[@id='J_login_form']/dl/dt/input[@id='J_password']")

如果某一个属性不足以定位时，可以加入多个条件并配以逻辑运算，

xxx.find_element_by_xpath("//input[@type='name' and @name='kw1']")

## 文本定位

当标签很少，但存在唯一的文本时可以使用文本来定位。

xxx.find_element_by_xpath("//标签[contains(text(),'文本值')]") 

## 模糊定位

属性值太长时，可以采用

xxx.find_element_by_xpath(“//a[contains(@href, ‘logout’)]”)

href属性中包含"logout"关键字的a标签。

## 动态属性定位

XPath 关于网页中的动态属性的定位，例如，ASP.NET应用程序中动态生成id属性值，可以有以下四种方法：

a.starts-with

例子： input[starts-with(@id,'ctrl')]

 解析：匹配以ctrl开始的属性值



 b.ends-with

例子：input[ends-with(@id,'_userName')]

解析：匹配以userName结尾的属性值



 c.contains() 

例子：Input[contains(@id,'userName')]

解析：匹配含有userName属性值

**和模糊定位一样**

## 节点关系定位

利用其兄弟节点或者父节点等各种可以定位的元素进行定位

## 一个简单的例子

```python
# coding=utf-8

from selenium import webdriver

driver = webdriver.Chrome('C:\Windows\System32\chromedriver')
#open the Homepage
driver.get("http://www.baidu.com")

# Find element -> pass keyword
try:
    driver.find_element_by_xpath("//input[@name='wd']").send_keys('selenium')
    # driver.find_element_by_id('kw').send_keys('selenium')
    print('find by id success!')
except:
    print('find by id error!')

#click to search
try:
    driver.find_element_by_xpath("//input[@type='submit']").click()
    # driver.find_element_by_id('su').click()
    print('click success!')
except:
    print('click error!')
```

# reference

【windows7】pythonでChromeのブラウザ操作を自動化 　～Selenium WebDriverを利用

(使用了unittest模块，**值得学习**)

https://qiita.com/ynishimura0922/items/a3332b5beb394248e44e

python selenium xpath定位方式

http://blog.csdn.net/huiseqiutian/article/details/73739707

[python爬虫] Selenium常见元素定位方法和操作的学习介绍

http://blog.csdn.net/eastmount/article/details/48108259