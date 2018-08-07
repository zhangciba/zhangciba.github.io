---
title: Debian下配置网络的方法
date: 2016-06-15 09:47:34
categories: Linux
tags: Debian网络
---

配置网卡
修改 /etc/network/interfaces 添加如下
```
# #号后面是备注
auto eth0 #开机自动激活
iface eth0 inte static #静态IP
address 192.168.0.56 #本机IP
netmask 255.255.255.0 #子网掩码
gateway 192.168.0.254 #路由网关
 
#因为我是通过路由上网的，所以配置为静态IP和网关
```

<!--more-->
如果是用DHCP自动获取，请在配置文件里添加如下：
```
iface eth0 inet dhcp
```

设置DNS
```
echo "nameserver 202.96.128.86" >> /etc/resolv.conf
#请设置为你当地的DNS
```

到这里配置好以后，重启一下网卡。
```
/etc/init.d/network restart
```
