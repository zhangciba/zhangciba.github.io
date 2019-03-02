---
title: 修改SSH服务端口号
date: 2016-05-23 12:32:06
categories: Linux
tags: ssh
---

### 首先修改配置文件
vi /etc/ssh/sshd_config
找到#Port 22一段，这里是标识默认使用22端口，修改为如下：
Port 22
Port 50000
然后保存退出
<!-- more -->
### 重启SSH服务
执行
```
/etc/init.d/sshd restart
```
这样SSH端口将同时工作与22和50000上。
<!-- more -->

### 开放端口
备注，使用了防火墙软件的需要在防火墙服务上面开启50000端口，否则会导致无法连接上。
#### iptables
编辑防火墙配置：
``` 
vi /etc/sysconfig/iptables
-A INPUT -p tcp -m tcp --dport 50000 -j ACCEPT
/etc/init.d/iptables restart
```

#### firewall
如果使用的是firewalld,执行如下命令开放50000端口
```
firewall-cmd --add-port=50000/tcp --permanent
#--permanent是永久开放，重启之后还会有效，不加，重启之后就会关闭。
```

### 测试连接
现在请使用ssh工具连接50000端口，来测试是否成功。
```
ssh -p 50000 root@yourhostname
 #-p指定连接端口
```
如果连接成功了，则再次编辑sshd_config的设置，将里边的Port22删除（dd掉），即可。
之所以先设置成两个端口，测试成功后再关闭一个端口，是为了方式在修改conf的过程中，
万一出现掉线、断网、误操作等未知情况时候，还能通过另外一个端口连接上去调试
以免发生连接不上必须派人去机房，导致问题更加复杂麻烦。
