---
title: HPC集群运维总结5-Lustre维护
date: 2017-12-12 20:20:25
categories: 集群管理
tags: [集群管理,运维,Linux,HPC]
---

[TOC]


### Lustre维护
<span id = "Lustre"></span>
科研教育网格集群采用了高性能分布式计算文件系统Lustre，Top500超级计算机中有超过50%都是用了该文件系统。该文件系统的命运特别坎坷，先后转手了好几家公司，现在是Intel公司所属。Lustre原先是一套开源的产品，最初是美国能源部提出开发的，后来成立CFS公司，2007年转卖给Sun公司，2010年Sun公司被Oracle公司收购，2010年又卖给了Whamcloud公司，最后于2012年被Intel收购。
<!-- more -->
Lustre是一种基于对象存储的分布式文件系统，主要包含MDS(Metadata Server)和OSS(Object Storage Server)，实验室集群的node309和node310是MDS,311-323是OSS，其中309和310互为备份，310提供了/public目录的元数据，并备份node309中的元数据，309之前提供了/home目录，后来因为一些故障，导致309没有再次提供服务了，目前309仅仅作为/public元数据的一个备份，但是309和310必须要同时启动才能正常工作。之前311-318提供/home目录的数据文件存储，319-323提供/public目录的数据文件存储，但是由于之前出现过阵列故障，所以目前只有319-323提供了数据存储服务。总结，node309和node310提供元数据存储服务，node329-323提供数据存储服务，这几个服务器不能随意允许个人登录，也不允许任意关机，否则导致整个集群无法正常使用。

所有计算节点都预先安装了Lustre客户端软件，使用Lustre文件系统，需要保证内核中加载了Lustre文件系统网络模块，配置文件在/etc/modprobe.d/lustre.conf，其中的内容如下：
```
options lnet networks="o2ib0(ib0),tcp0(eth0)"
```
Lustre文件系统的网络模块，其中配置了两种网络通道，第一种是Infiniband网络通道，另外一种是以太网通道。如果节点的InfiniBand网络通道出现问题，可以使用以太网，后面会介绍Infiniband网络出现问题时使用以太网挂载Lustre文件系统的方法。

Lustre文件系统的挂载脚本是/etc/client_mount.local内部，内容如下
```
sleep 20
mount -t lustre "inode310@o2ib0:inode309@o2ib0:node310@tcp0:node309@tcp0:/p100fs02" /public
#mount -t lustre "inode309@o2ib0:inode310@o2ib0:node309@tcp0:node310@tcp0:/p100fs01" /home
mount --rbind /public/home /home
```

由于Lustre网络模块或者Infiniband网络模块还没有初始化，因此需要等待20秒之后再次挂载，有些节点可能由于InfiniBand网络加载速度更慢，因此需要等待更长时间才能执行挂载工作。由于集群之前的/home目录出现问题，因此现在将/public/home目录挂载为/home，也就是说/home目录实际上也是由/public目录存储的。

如果某个节点无法挂载Lustre文件系统，登录到节点，首先检查节点的Infiniband网络是否正常，执行如下命令中的一种，
```
[root@node236 ~]# ibstat
CA 'mlx4_0'
	CA type: MT4099
	Number of ports: 1
	Firmware version: 2.30.3000
	Hardware version: 0
	Node GUID: 0x46d2c92000003890
	System image GUID: 0x46d2c92000003893
	Port 1:
		State: Active
		Physical state: LinkUp
		Rate: 56
		Base lid: 151
		LMC: 0
		SM lid: 317
		Capability mask: 0x02514868
		Port GUID: 0x46d2c92000003891
		Link layer: InfiniBand
```

```
[root@node236 ~]# ibstatus
Infiniband device 'mlx4_0' port 1 status:
	default gid:	 fe80:0000:0000:0000:46d2:c920:0000:3891
	base lid:	 0x97
	sm lid:		 0x13d
	state:		 4: ACTIVE
	phys state:	 5: LinkUp
	rate:		 56 Gb/sec (4X FDR)
	link_layer:	 InfiniBand
```

如果state属性显示ACTIVE，那么表示Infiniband网络是好的，可以手动进行挂载，如果挂载还是出现问题，很有可能是Lustre文件系统模块没有加载完成，另外还需要执行uname-a命令查看内核的版本是否被用户升级或者更换了。如果内核不是2.6.32-220，那么不能使用Lustre文件系统。因为Lustre客户端没有在其他内核版本进行安装。
```
[root@node236 ~]# uname -a
Linux node236 2.6.32-220.el6.x86_64 #1 SMP Wed Nov 9 08:03:13 EST 2011 x86_64 x86_64 x86_64 GNU/Linux
```

如果Infiniband网络的Physical State属性显示是PortConfigurationTraining同时State是Down，这种情况一般是Infiniband网络的子网管理服务器没有启动的原因，可以使用执行如下命令重启Infiniband网络服务，然后再查看Infiniband网络是否正常。
```
service openibd restart
service opensmd restart
```

如果phys state是Down，那么很有可能机器的Infiniband没有安装或者存在问题，那么试试重新安装驱动，如果还是没有效果，那么很有可能Infiniband线缆已经损坏，可以从刀箱后面拔掉坏掉的线缆，插上其他的线缆试试，一般线缆是好的，Infiniband网络的网口会有两个灯亮着，如果灯不亮，一般有可能是驱动没有安装，也有可能是线缆坏掉了，或者是Infiniband线缆坏掉了。

备注：用户重新安装操作系统后，无法使用Lustre文件系统。

#### 使用以太网挂载Lustre文件系统
如果确定Infiniband网络线缆有问题，那么可以不用Infiniband网络，而是直接使用以太网络来进行Lustre通信。方法如下，将/etc/modprobe.d/lustre.conf修改为如下内容：

```
options lnet networks="tcp0(eth0)"
```

然后将挂载脚本/etc/client_mount.local文件修改为如下内容
```
sleep 20
mount -t lustre "node310@tcp0:node309@tcp0:/p100fs02" /public
mount --rbind /public/home /home
```
最后重新启动计算节点，不出意外情况，应该可以正常使用Lustre文件系统了，如果还是有问题，只有两种可能，一种是Lustre模块出现问题，需要重新配置，另外一种可能是，内核中没有安装Lustre文件系统。

