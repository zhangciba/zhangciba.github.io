---
title: CentOS7安装Mono和MonoDevelop
date: 2016-04-11 10:50:03
tags: Mono
categories: Linux
---

MonoDevelop 是个Linux平台上的开放源代码集成开发环境，主要用来开发Mono与.NET Framework软件。MonoDevelop 整合了很多Eclipse与Microsoft Visual Studio的特性，像是 Intellisense、版本控制还有 GUI 与 Web 设计工具。另外还整合了GTK# GUI设计工具(叫做Stetic)。目前支持的语言有C#、Java、BOO、Nemerle、Visual Basic .NET、CIL、C与C++ 。

简单明了的讲解下载CentOS7 下安装Mono 和 MonoDevelop过程。

本次所有操作在root模式下
1.执行  
```
rpm --import "http://keyserver.Ubuntu.com/pks/lookup？op=get&search=0x3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF"
```
<!-- more -->
2.执行  
```
yum-config-manager --add-repo http://download.mono-project.com/repo/centos/
```
步骤1-2是添加mono安装使用的资源环境

3.执行 yum install mono  按照提示安装所以安装包

4.安装libgdiplus
```
　　mkdir /var/local/src　　　创建文件夹
　　cd /var/local/src　　　　　进入创建文件
　　wget http://download.mono-project.com/sources/libgdiplus/libgdiplus-3.12.tar.gz     下载文件
　　tar -zxvf libgdiplus-3.12.tar.gz　解压下载文件
　　cd libgdiplus-3.12　　　　　进入解压文件夹
　　./configure　　　　　对安装程序进行配置
　　make && make install　　编译并安装
```
5.安装gtk-sharp
```
　  cd /var/local/src　进入创建文件
　　wget http://download.mono-project.com/sources/gtk-sharp212/gtk-sharp-2.12.26.tar.gz　　下载文件
　　tar -zxvf gtk-sharp-2.12.26.tar.gz　 解压下载文件
　　cd gtk-sharp-2.12.26　　进入解压文件夹
　　./configure　　　　 对安装程序进行配置
　　make && make install　编译并安装
```
6.安装monodevelop 
 	```
 	yum install monodevelop
 	```
安装完成就OK了。


