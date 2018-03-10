---
title: Crawler-01-enviroment
date: 2018-03-10 12:17:47
categories: Crawler
tags:
- Selenium
- Install
---



selenium模块可以模拟浏览器的操作来实现数据的抓取，这种方式可以有效避免网站屏蔽等问题。

成功抓取需要selenium模块以及浏览器对应的驱动模块。下面介绍所需环境的准备工作。

# selenium installation

可以通过pip命令安装，

```python
pip install selenium
```

# Web Driver

Firefox浏览器默认支持selenium，不需要下载Driver。

```python
# 不需要Path参数
driver = webdriver.Firfox()
```

下载浏览器对应的Driver，chrome浏览器可以到http://chromedriver.storage.googleapis.com/index.html下载合适版本。

```python
# 需要指定Driver的保存位置
driver = webdriver.Chrome('C:\...\chromedriver')
```

