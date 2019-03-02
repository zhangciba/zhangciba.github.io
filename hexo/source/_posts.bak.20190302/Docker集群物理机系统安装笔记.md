---
title: Docker集群物理机系统安装笔记
date: 2016-04-07 20:48:56
tags: Docker
categories: 云计算
---

首先在物理机器上安装CentOS7，安装类型选择为计算节点。

整体配置过程
* 修改IP地址，重启网络服务;
* 配置锐捷网络客户端，连接互联网;
* 关闭SELinux、firewalld等服务;
* 修改主机名 hostnamectl set-hostname <主机名>;
* 配置集群内部主机间ssh公钥登录;
* 同步hosts文件、修改vimrc、virc(在/etc/目录下);
* 配置yum epel源，yum不更新内核

安装系统之后的配置过程：
### 1.配置IP地址，修改默认启动网卡
```
sudo vi /etc/sysconfig/network-scripts/ifcfg-ens33
```
关闭网卡，并重启网卡，ens33是我的网卡的名称
```
ifdown ens33
ifup ens33
```
<!-- more -->

### 2.安装锐捷客户端，并配置开机联网

#将锐捷客户端拷贝到usr目录下
```
sudo cp -r /run/meida/zhangchi/ZHANGHI/Su /usr/local/Su
sudo chmod -R 755 /usr/local/Su
```

#将联网的服务脚本拷贝到开机启动目录，并赋予执行权限
```
sudo cp /run/media/zhangchi/ZHAGNCHI/rjnetwork /etc/init.d/ 
sudo chomod -R 755 /etc/init.d/rjnetwork
sudo vi /etc/init.d/rjnetwork
```
注：rjnetwork是我编写的使用锐捷认证客户端进行校园网连接的自启动脚本，功能就是每次系统开机之后首先进行网络连接。其代码如下：
```
#!/bin/sh
#chkconfig:2345 80 90 设置何种模式下进行启动，以及开机启动顺序以及关机关闭顺序。
#description:login to internet 对该服务的描述
#processname:rjnetwork 进行名称
cd /usr/local/Su
sh rjsupplicant.sh -a 1 -d 0 -u yourusernameXXX -S 1 -p yourpasswordXXX
```

#将联网的服务配置为开机启动
```
sudo chkconfig --add /etc/init.d/rjnetwork
sudo chkconfig rjnetwork on
```

### 关闭SELinux
```
sudo vi /etc/selinux/config
将SELINUX=enforcing改为SELINUX=disable
```

### 开启sshd服务
```
sudo systemctl enable sshd
sudo systemctl start sshd
```

### 开放22号端口
前提条件是你使用的是firewalld，如果使用iptables作为防火墙，则使用iptables的添加规则。
```
firewall-cmd --zone=public --add-port=22/tcp --permanent
firewall-cmd --reload
```

### 重启系统之后默认联网，并开放了SSH服务
```
reboot
```

