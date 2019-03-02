---
title: 'Cannot retrive metalink for repository:epel.Please verify its path and try again'
date: 2017-02-09 17:25:28
categories:
tags:
---

yum安装时出现：Cannot retrieve metalink for repository: epel. Please verify its path and try again
在CentOS 6.3 x86_64下安装php-mcrypt的时候出现了问题：Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again，需要安装epel源。

解决方法： 一句话：把/etc/yum.repos.d/epel.repo，文件第3行注释去掉，把第四行注释掉。具体如下：
<!--more-->

打开/etc/yum.repos.d/epel.repo，将

[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
修改为

[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
再清理源，重新安装

yum clean all
yum install -y 需要的包
 

如果还是不行，修改DNS,到/etc/resolv.conf下添加一下：

nameserver 8.8.8.8
search localdomain

然后重启network服务：service network restart
