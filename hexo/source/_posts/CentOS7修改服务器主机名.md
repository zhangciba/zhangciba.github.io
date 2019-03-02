---
title: CentOS7修改服务器主机名
date: 2016-06-02 17:27:25
categories: Linux
tags: 主机名
---

CentOS7修改服务器主机名方法

CentOS7下修改主机名

第一种：hostname 主机名
```
hostname 主机名称 
```

这种方式，只能修改临时的主机名，当重启机器后，主机名称又变回来了。

第二种：hostnamectl set-hostname <hostname>

命令行中输入
```
hostnamectl set-hostname <主机名> 
```
使用这种方式修改，可以永久性的修改主机名称

