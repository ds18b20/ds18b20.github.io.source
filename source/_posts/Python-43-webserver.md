---
title: Python-43-webserver
date: 2018-06-24 23:26:07
categories: Python
tags:
- web server

---

# simple web server

使用python自带的http模块可以快速地实现http web server。

在目录下执行`python -m http.server 80`，如果该目录下已经有index.html文件的话，在浏览器内输入`localhost`或者`localhost:80`即可看到index.html的内容。

![](Python-43-webserver\webserver.JPG)

结束时，按下Ctrl+C来终止服务。

# 参考

[python3 使用SimpleHTTPServer搭建web服务器](https://www.cnblogs.com/harry-xiaojun/p/6739003.html)

[Python3实现简单的http server](https://www.cnblogs.com/asis/p/6842996.html)

