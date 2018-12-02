---
title: web-flask-gunicorn
date: 2018-11-08 22:44:13
categories: web
tags:
- flask
- gunicorn
- deployment

---

# 环境准备

Flask提供用python写的HTTP应用；

gunicorn是一个UNIX下的WFGI HTTP服务器

## 安装python3

```
sudo apt-get update
sudo apt-get install python3.6
```

查看版本：

```
python3 --version
# or
python3 -V
```

## 安装pip3

aws的EC2平台提供的ubuntu系统默认自带python2。直接安装pip的话会安装成pip2，这样使用pip继续安装各种包的话就会安装到python2下面。我们希望使用python3来写app，所以首先要安装对应的pip3：

# flask

```
sudo apt-get install python3-pip
```

https://stackoverflow.com/questions/6587507/how-to-install-pip-with-python-3

## 安装nginx

```
sudo apt-get install nginx
```

Nginx的配置在下面的介绍

## 安装virtualenv

在原生环境下直接安装包的话，由于各种已存在的设置的影响，有时候会出现各种莫名其妙的问题。比如在py2和py3共存的环境下，运行脚本时很可能出现混乱。因此，所有的新app都应该在一个干净的环境下开发和部署，这时就可以使用virtualenv包来从零新建一个独立空间。

```
sudo apt-get install python-virtualenv
```

https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04

## 使用virtualenv

切换到工作目录：

```
cd /var/www
sudo mkdir appname
```

新建venv，使用-p参数指定python版本：

```
virtualenv -p /usr/bin/python3.6 venv
```

激活venv：

```
source venv/bin/activate
```

成功的话，在辞职后所有的命令行前都有（venv）标记，表示在venv下工作。

然后在此venv下安装各种包，这样安装的包不会影响到外部环境。

- 安装flask：

```
pip install flask
```

- 安装Gunicorn

```
pip install gunicorn
```

- 根据requirements.txt安装项目依赖

```
pip install -r requirements.txt
```

# 配置Nginx

端口转发：外部80端口的请求转发到127.0.0.1:5000上；

配置过程为：

```
sudo touch /etc/nginx/sites-available/setting_file_name
sudo vim setting_file_name
```

以下是一个示例，具体参见另一篇介绍Nginx配置的文章。

```
# file_name=default
server {
    listen 80;
    server_name example.org; # 这是HOST机器的外部域名，用地址也行

    location / {
        proxy_pass http://your.私有IP:5000; # 指向 gunicorn host 的服务地址，注意，这里填我们服务器的私有IP
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
```

> 注：vim的命令
>
> i：开始编辑模式
>
> Esc：退出编辑模式
>
> :wq：保存并退出vim

软连接到sites-enabled目录下：

```
sudo ln -s /etc/nginx/sites-available/setting_file_name /etc/nginx/sites-enabled/setting_file_name
```

注意一定要重启Nginx才能使更改生效：

```
# ngnix syntax test
sudo nginx -t

# restatrt nginx
sudo /etc/init.d/nginx restart
```

# 启动项目

使用gunicorn启动app，并保持app应用的维持。

- 简单快速启动：

```
gunicorn filename:app
```

这里的filename是我们的入口模块名，app是flask实例名称。

- 参数启动：

```
gunicorn -w 4 -b 127.0.0.1:5000 filename:app
```

- 后台启动

```
nohup gunicorn -w 4 -b 127.0.0.1:5000 filename:app&     //关闭远程连接时程序在后台继续运行
```

用四个 worker 进程来运行一个 Flask 应用（ -w 4 ），绑定到 localhost 的5000 端口（ -b 127.0.0.1:5000 ）

# 其他

退出虚拟环境：

```
deactivate
```

关闭gunicorn

```
pkill gunicorn  //关闭gunicorn
```

查看gunicorn进程：

```
pstree -ap|grep gunicorn
```

# 参考

通过Gunicorn部署flask应用（阿里云服务器：Ubuntu 16.04）

https://juejin.im/post/5a5a1408518825733060e232

