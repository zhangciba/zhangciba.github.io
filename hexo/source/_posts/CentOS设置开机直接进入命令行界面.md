---
title: CentOS设置开机直接进入命令行界面
date: 2016-04-08 08:46:48
tags: CentOS
categories: Linux
---

上网查询CentOS设置开机直接进入命令行界面的方法都说修改/etc/inittab文件，将文件中的“ :id:5:initdefault:”改为“ :id:3:initdefault:”，即将默认的runlevel由5改为3，但在CentOS7下打开/etc/inittab页面显示如下：
```
# inittab is no longer used when using systemd.
#
# ADDING CONFIGURATION HERE WILL HAVE NO EFFECT ON YOUR SYSTEM.
#
# Ctrl-Alt-Delete is handled by /etc/systemd/system/ctrl-alt-del.target
#
# systemd uses 'targets' instead of runlevels. By default, there are two main targets:
#
# multi-user.target: analogous to runlevel 3
# graphical.target: analogous to runlevel 5
#
# To set a default target, run:
#
# ln -sf /lib/systemd/system/<target name>.target /etc/systemd/system/default.target
#
```
<!-- more -->
提示说inittab文件已经不被使用，现在CentOS7使用systemd作为新的init系统，而systemd系统使用“target”来代替“runlevel”，默认有两个主要的target：

multi-user.target：相当于runlevel 3[命令行界面]，graphical.target：相当于runlevel 5[图形界面]

设置默认的target则使用命令：
```
ln -sf /lib/systemd/system/<target name>.target /etc/systemd/system/default.target
```

我这里要将CentOS7开机默认进入命令行界面，则运行命令：
```
# ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
```


