---
title: Java-00-Install Eclipse
date: 2018-03-07 23:23:00
categories: Java
tags:
- Java
- eclipse
- install
---

This article shows how to install eclipse on ubuntu.

## Download & unzip

Download installation from home page, and unzip file.

```
sudo tar zxvf eclipse-jee-mars-2-linux-gtk-x86_64.tar.gz -C /usr/***/ 
```



# Install

Double click unzipped file *eclipse_inst* to start installation.

Select directory to install.



# Add icon to dash

```
# change to dash directory
cd /usr/share/applications

# add a new file: eclipse.desktop
# and edit
sudo gedit eclipse.desktop
```

Add these contents:

```
[DesktopEntry]

Encoding=UTF-8

Name=Eclipse

Comment=Eclipse

Exec=*Your installation path*

Icon=*Your icon path*

Terminal=false

StartupNotify=true

Type=Application

Categories=Application;Development;
```

Then, make it excutable,

```
sudo chmod u+x eclipse.desktop
```



# reference

Ubuntu16.04LTS傻瓜式安装Eclipse图文详情加注释

http://blog.csdn.net/qq_37549757/article/details/56012895