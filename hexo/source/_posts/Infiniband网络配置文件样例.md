---
title: Infiniband网络配置文件样例
date: 2017-11-02 16:16:45
categories: 集群管理
tags: [Infiniband, IB]
---

CentOS7.2 IB网络配置
ifcfg-ib0
```
MTU=65520
CONNECTED_MODE=yes
TYPE=InfiniBand
BOOTPROTO=static
IPADDR=10.10.0.80
PREFIX=16
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=ib0
DEVICE=ib0
ONBOOT=yes
```

Redhat Enterprise Linux 6.2
```
DEVICE="ib0"
IPADDR="10.10.0.156"
NETMASK="255.255.0.0"
BOOTPROTO="static"
ONBOOT="yes"
```
