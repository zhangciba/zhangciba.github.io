---
title: 修改CentOS7 GRUB
date: 2016-06-07 20:20:45
categories: Linux
tags: grub
---

修改GRUB改变启动顺序是作为一个Linux运维人员必备的技能，特别是内核升级失败的情况下。
CentOS官方的文档讲解的很详细，可以参考一下
[在 CentOS 7 上设置 grub2](https://wiki.centos.org/zh/HowTos/Grub2)

<!--more-->
下面是一个老外的博客上面写的，也可以实现，就是自己修改默认的GRUB的值
Change default grub startup in CentOS 7
This post describes how to change the default startup choice while booting up CentOS 7.

Edit the file /etc/default/grub

Change the line
```
GRUB_DEFAULT=saved
```
to
```
GRUB_DEFAULT="Windows 7 (loader) (on /dev/sda1)"
```

The value to add is determined by looking at the menuentry lines in /boot/grub2/grub.cfg
Choose the menuentry that you would like to be default.

To make this change active run the command
```
# grub2-mkconfig -o /boot/grub2/grub.cfg
```
You will see this change the next time you reboot.
