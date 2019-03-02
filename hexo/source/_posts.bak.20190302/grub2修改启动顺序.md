---
title: grub2修改启动顺序
date: 2018-08-28 15:56:47
categories: Linux
tags: grub2
keywords: grub2启动顺序
description:
---

1. 查看所有的entry
```
[root@dpdk grub2]# awk -F \' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
0 : CentOS Linux (3.10.0-693.11.1.el7.x86_64) 7 (Core)
1 : CentOS Linux (3.10.0-693.5.2.el7.x86_64) 7 (Core)
2 : CentOS Linux (3.10.0-693.2.2.el7.x86_64) 7 (Core)
3 : CentOS Linux (0-rescue-37138ca794604b28bca5b6394f5cd3c2) 7 (Core)
```

2. 查看当前default的entry
```
[root@dpdk grub2]# grub2-editenv list
saved_entry=CentOS Linux (3.10.0-693.11.1.el7.x86_64) 7 (Core)
[root@dpdk grub2]#
```

3. 修改为指定的entry
```
[root@dpdk grub2]# grub2-set-default 2
[root@dpdk grub2]# grub2-editenv list
saved_entry=2
[root@dpdk grub2]#
```
