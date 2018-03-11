---
title: Java-00-Install Java
date: 2018-03-07 23:23:00
categories: Java
tags:
- Java
- install
---

# install java on ubuntu

You have 2 ways to install java on you ubuntu machine.

## by ppa(源)

```
# add ppa
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
```

```
# install oracle-java-installer
sudo apt-get install oracle-java8-installer
```

## set default version

set jdk8 as your default jdk:

```
sudo update-java-alternatives -s java-8-oracle
```

If you already have a  jdk7 and installed jdk8, you can change between 7 and 8 by

```
# jdk8 切换到jdk7
sudo update-java-alternatives -s java-7-oracle
```

```
# jdk7 切换到jdk8
sudo update-java-alternatives -s java-8-oracle
```

Test if jdk is installed properly,

```
java -version
javac -version
```

# by downloading package

1. download .gz package from oracle HP.

2. make directory

   ```
   sudo mkdir /usr/lib/jvm
   ```

   unzip

   ```
   sudo tar -zxvf jdk-7u60-linux-x64.gz -C /usr/lib/jvm
   ```

3. modify enviroment variable

   ```
   sudo gedit ~/.bashrc
   ```

   add,

   ```
   #set oracle jdk environment
   export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_60  ## 这里要注意目录要换成自己解压的jdk 目录
   export JRE_HOME=${JAVA_HOME}/jre  
   export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
   export PATH=${JAVA_HOME}/bin:$PATH 
   ```

   make modification effective,

   ```
   source ~/.bashrc
   ```

4. change default jdk version

   ```
   sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_60/bin/java 300  
   sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_60/bin/javac 300  
   sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk1.7.0_60/bin/jar 300   
   sudo update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/jdk1.7.0_60/bin/javah 300   
   sudo update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/jdk1.7.0_60/bin/javap 300  
   ```

   and

   ```
   sudo update-alternatives --config java
   ```

   If meesages are shown like bellow:

   ```
   There is only one alternative in link group java (providing /usr/bin/java):
   /usr/lib/jvm/jdk1.7.0_60/bin/java
   ```

   there is no need to change version.

5. test

   ```
   java -version
   ```


# install on windows

参考这篇文章：

Windows环境下JDK安装与环境变量配置详细的图文教程

http://www.cnblogs.com/liuhongfeng/p/4177568.html

# reference

Ubuntu 安装 JDK8 的两种方式

https://www.cnblogs.com/smiler/p/6939913.html

修改linux默认jdk版本

https://www.cnblogs.com/onetwo/p/5095481.html

