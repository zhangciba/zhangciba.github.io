---
title: Intel-fortran编译器ifort安装
date: 2017-05-06 08:40:58
categories: 集群管理
tags: ifort
---

1 下载ifort，网址：http://software.intel.com/en-us/articles/non-commercial-software-download/
2 首先查看操作系统的一些库是否安装，其中有gcc，g++,build-essential，打开命令行，进行以下操作：
	```
	sudo apt-get install gcc
	 sudo apt-get install build-essential
	 sudo apt-get install g++
	 ```
3 安装amd64版本的编译器也需要一些32位库支持，使用命令安装：
	```
	sudo apt-get install ia32-libs
	```
4 之后安装其它一些32位的库，在命令行进行如下操作：
	```
	sudo apt-get install libstdc++5
	sudo apt-get install lib32stdc++6
	sudo apt-get install libc6-dev-i386
	sudo apt-get install gcc-multilib
	sudo apt-get install g++-multilib
	```
5 需要安装rpm包：`sudo apt-get install rpm`
<!--more-->
6 安装JRE
	*	`sudo apt-get install openjdk-6-jre-headless`
	*	`java –version`
7 安装intel fortran
	*	cd到intel fortran所在目录
	*	`sudo ./install.sh`
8 设置系统变量
	*	`vim .bashrc`
	*	在打开的文件末尾加上一行：`source /opt/intel/bin/ifortvars.sh intel64`
	*	保存，关闭.bashrc
	*	对.bashrc文件进行更新：`source .bashrc`
9 试验安装是否成功：`ifort –v`

注意：在进行到第7步的时候，有可能会出现一下错误提示：“sh: Syntax error: Bad fd number”。需要对这个错误进行处理，否则在安装结束后，设置系统变量时会出错。 原因为sh链接到了dash，而正确的应该链接到bash，具体处理方法为:
	```
	ls -l /bin/sh
  #查看是不是真的链接到了dash，如果链接到了dash，命令行会显示：
	lrwxrwxrwx 1 root root 4 Sep 25 14:48 /bin/sh -> dash
	sudo dpkg-reconfigure -plow dash
	#出现菜单询问是否链接到dash，选no
	
	ls -l /bin/sh
	#修改过后，再查看一下，如果显示：
	lrwxrwxrwx 1 root root 4 Sep 25 14:48 /bin/sh -> bash
	#说明已经修改成功
   ```
继续进行安装ifort


