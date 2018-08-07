---
title: HPC集群运维总结4-网络访问控制
date: 2017-12-12 20:20:24
categories: 集群管理
tags: [集群管理,运维,Linux,HPC]
---

[TOC]



### 网络访问控制
集群主要通过安全网关硬件和iptables进行访问控制。

安全网关配置了允许登录进入集群的IP类型以及开放的端口信息。
具体的IP地址限制由iptables来进行限制。

#### 允许某一IP地址访问集群
编辑/etc/sysconfig/iptables文件，
```
-A INPUT -s 99.99.1.1 -p tcp -m tcp -j ACCEPT
```
<!-- more -->
然后执行如下命令，重启iptables服务，
```
service iptables restart
```
即可允许99.99.1.1这个IP地址登录到集群。

#### 允许网段的IP地址访问集群
编辑/etc/sysconfig/iptables文件，
```
-A INPUT -s 99.99.1.1/24 -p tcp -m tcp -j ACCEPT
```

然后执行如下命令，重启iptables服务，
```
service iptables restart
```

即可允许99.99.1.1-99.99.1.255这个网段内部的所有IP地址登录集群。

#### 允许所有节点访问特定的IP地址
编辑/etc/sysconfig/iptables文件，
```
-A POSTROUTING -d 202.114.0.242/32 -o eth1 -j MASQUERADE
```

然后执行如下命令，重启iptables服务，
```
service iptables restart
```
执行完命令后，所有集群都可以访问202.114.0.242这个IP地址所在的服务器。
此时，需要在需要访问202.114.0.242这个IP的计算节点上面执行下面的操作。
首先登录到需要访问外网的节点，执行如下命令：
```
route
```

查看结果中是否有以下记录：
```
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         node307         0.0.0.0         UG    0      0        0 eth0
```

如果存在上述内容，则直接可以连接外网，如果ping外网还是不同，一般有可能是DNS信息没有填写正确，可以编辑/etc/resolv.conf文件，在该文件头部加入一下信息。
```
nameserver 202.114.0.242
nameserver 202.112.0.131
```
保存后，就可以正常访问外部网络了。

#### 允许某一节点访问特定的IP地址
编辑/etc/sysconfig/iptables文件，
```
-A POSTROUTING -s 11.11.0.1/32 -d 192.30.252.128/32 -o eth1 -j MASQUERADE
```

然后执行如下命令，重启iptables服务，
```
service iptables restart
```
执行完命令后，允许node1（IP地址为11.11.0.1）访问github网站（IP地址为192.30.252.128）。

#### 允许某一节点访问所有网站
编辑/etc/sysconfig/iptables文件，
```
-A POSTROUTING -s 11.11.0.1/32 -o eth1 -j MASQUERADE
```

然后执行如下命令，重启iptables服务，
```
service iptables restart
```
执行完命令后，node1（IP地址为11.11.0.1）具有全部外网访问权限。

#### 配置端口转发规则
允许集群外部访问内部节点的某一或多个端口，实际原理是SNAT和DNAT配置。
例如说。我希望访问集群node1(IP地址为11.11.0.1)的http服务(端口号为80)，此时管理节点的IP为99.99.1.1(虚构的一个外网IP地址)，则配置如下:

```
-A PREROUTING --dst 99.99.1.1 -p tcp --dport 8888 -j DNAT --to-destination 11.11.0.1:80
-A POSTROUTING -d 11.11.0.1 -p tcp -m tcp --dport 80 -j SNAT --to-source 99.99.1.1
```

以上语句的意思是，将访问99.99.1.1:8888的数据包都转发到11.11.0.1:80，

#### 集群DNS服务器信息
以下是校内DNS服务器的信息，可以编辑/etc/resolv.conf文件进行设置
```
nameserver 202.114.0.242
nameserver 202.112.0.131
```

### 网络故障处理

#### Infiniband网络故障
<span id="Infiniband"></span>
[Mellanox Infiniband驱动下载地址](http://www.mellanox.com/page/software_overview_ib)
上面是Infiniband驱动的官方下载地址，根据系统版本进行下载。

Infiniband网络最多的故障就是状态不是Active，导致状态一直是PortConfigurationTraining，这个时候可以稍等2分钟，如果状态一直是这样，可以考虑重启一下openibd服务，如果重启后还是不行，可能是子网管理服务没有启动，这种情况下，应该停止集群所有节点的子网管理服务，opensmd服务，**请务必记住，一个子网内部只能启动一个opensmd服务，启动多个会导致Infiniband网络出现问题，接着导致文件系统无法使用，直接导致集群无法正常计算**，因此需要登录到运行opensmd服务的节点，<span id = "ibpro">本集群默认在node310上开启opensmd服务，</span>命令如下：
```
service opensmd restart
```

如果提示服务不存在，那么问题的原因就是网络驱动没有正常安装，或者是出现了故障，这个时候可以采用重新安装驱动的方式来进行修复。

Infiniband网络配置文件为/etc/sysconfig/network-scripts/ifcfg-ib0

#### 以太网故障

以太网故障一般比较少，常见的故障可能是刀片机箱的交换模块断电了。交换模块断电的一个明显的特征是网络指示灯一个都不亮，这种情况在集群断电后首次启动时经常遇到。

<span id = "power_module_error"></span>
造成刀箱的网络交换模块无法正常工作的原因是，刀箱的电源模块在长时间断电之后没有正常启动或者由于电源模块功率过高而风扇转速太低导致电源过热保护，特点是指示灯为黄色，如果一个刀箱有2个电源模块亮黄灯，那么很有可能导致刀箱的交换模块无法正常启动。这个时候可以尝试一下办法来解决这个问题。

将亮黄灯电源模块的电源线拔下，将电源模块拔下，等待1-2分钟之后，黄灯熄灭，然后把电源模块重新插入，并插上电源模块的电源线，如果一会之后，电源模块指示灯亮绿色，表示该电源模块可以正常工作。将所有的电源模块都进行这样的处理。直到所有的电源模块指示灯都是绿色的。

然后将刀箱内部的所有刀片节点进行关机处理，关机后，在刀箱背面的左下角的有一个黑色的开关，这个开关是整个刀箱的电源开关，它控制刀箱所有模块的供电。找到后关闭它，等待5分钟后，重启打开该开关，如果不出意外情况，也就是所有的电源模块指示灯为绿色的情况下，一会刀箱的电源交换模块的网络指示灯就会正常亮起来了。如果还是不行，有可能就是交换模块出现故障了。

**如果不能正常解决，直接打电话给官方客服进行报修啦。**

