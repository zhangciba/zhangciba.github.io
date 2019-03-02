---
title: Linux SSH 远程登录后显示-bash-4.1$的解决办法
date: 2016-06-30 09:59:35
categories: ssh
tags: ssh
---

linux系统新建的用户用ssh远程登陆显示-bash-4.1$，不显示用户名路径。

### 产生这种问题有三种原因。
* 没有给用户创建/home/username主目录
* 用户没有/home/username主目录的读取权限
* 主目录/home/username下面缺少shell环境
<!--more-->

### 解决方法
* 创建主目录
```
mkdir /home/username
#将username换成用户名
```

* 修改用户的所有者和组
```
chown username:usergroup /home/username
#把username换成用户名
#把usergroup换成用户所在组
```

* 拷贝Shell环境到主目录下
```
cp -pr /etc/skel/.bash* /home/username/
# /home/username/是用户的主目录
```

