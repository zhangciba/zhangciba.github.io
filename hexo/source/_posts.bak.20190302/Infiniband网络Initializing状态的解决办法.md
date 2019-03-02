---
title: Infiniband网络Initializing状态的解决办法
date: 2017-04-27 15:56:29
categories: 集群管理
tags: Infiniband网络
---

如果Infiniband网卡驱动已经正常安装，但是执行
`ibstatus`命令或者`ibstat`命令时，Status字段总是显示Initializing状态，Physical State处于LinkUP状态，

物理状态显示的IB线缆是否已经正常连接到IB网卡上。如果显示LinkUP状态，则显示该主机的IB线缆已经借号了。

可以执行`service opensmd restart`命令重启一下子网管理服务，大部分时候，你会发现，IB网络恢复正常了。

