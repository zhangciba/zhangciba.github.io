---
title: iptables利用connlimit模块限制同一IP连接数
date: 2016-05-24 13:05:36
categories: Linux
tags: iptables
---

connlimit功能：
　　connlimit模块允许你限制每个客户端IP的并发连接数，即每个IP同时连接到一个服务器个数。
　　connlimit模块主要可以限制内网用户的网络使用，对服务器而言则可以限制每个IP发起的连接数。
 
connlimit参数：
　　–connlimit-above n 　　　＃限制为多少个
　　–connlimit-mask n 　　　 ＃这组主机的掩码,默认是connlimit-mask 32 ,即每个IP.

<!-- more -->
系统:centos 5.4 32位
  需要的软件包:iptables-1.3.8.tar.bz2 linux-2.6.18.tar.bz2 patch-o-matic-ng-20080214.tar.bz2 (这3个我都会提供给大家的)

### 准备工作
```
yum -y install gcc* make wget ncurses-devel ncurses bzip2
```

还有个重要的地方就是,如果你的系统安装iptables,那就先把iptables停一下,避免后面的问题
services iptables stop
然后把3个软件包解压到/usr/src目录下
```
tar jxf iptables-1.3.8.tar.bz2 -C /usr/src/
tar jxf patch-o-matic-ng-20080214.tar.bz2 -C /usr/src/
tar jxf linux-2.6.18.tar.bz2 -C /usr/src/
```

初始化内核
```
cd /usr/src/linux-2.6.18/
uname -r
2.6.18-164.el5
vi Makefile 改EXTRAVERSION =-164.el5
```

设置patch-o-matic-ng-20080214需要用到的环境变量:
```
export KERNEL_DIR=/usr/src/linux-2.6.18
export KERNEL_SRC=/usr/src/linux-2.6.18
export IPTABLES_SRC=/usr/src/iptables-1.3.8/
export IPTABLES_DIR=/usr/src/iptables-1.3.8/
```
这里说下,如果你不设置这4个环境变量的,后面你给内核下补丁和添加模块是很烦的一件事情,为什么这么说呢,让你们看个例子:
```
KERNEL_DIR=/usr/src/linux-2.6.18 IPTABLES_DIR=/usr/src/iptables-1.3.8 ./runme time
```
是不是很麻烦啊,所以最好是添加上.

### 给内核下补丁和添加模块
```
cd /usr/src/patch-o-matic-ng-20080214/
./runme --download
./runme connlimit
```
可以看到connlimit模块已经添加到内核里,但还没有完.

### 编辑内核配置文件,选中新增加的模块
```
cd ../linux-2.6.18/
make menuconfig
```

在内核配置界面选中
```
Networking --->
Networking options --->
Network packet filtering (replaces ipchains) --->
IP: Netfilter Configuration --->
<M> Connections/IP limit match support
```

### 编译内核模块
```
make modules_prepare
```

修改net/ipv4/netfilter/Makefile,只编译connlimit模块,首先备份net/ipv4/netfilter/Makefile文件
```
mv net/ipv4/netfilter/Makefile net/ipv4/netfilter/Makefile.bak
```

新建 net/ipv4/netfilter/Makefile文件，并添加如下内容
```
vi net/ipv4/netfilter/Makefile
obj-m := ipt_connlimit.o

KDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)

default:
$(MAKE) -C $(KDIR) M=$(PWD) modules
```

最后编译内核模块
```
make M=net/ipv4/netfilter/
```

### 将编译好的ipt_connlimit.ko内核模块复制到当前内核模块目录下,并加载内核模块
cp net/ipv4/netfilter/ipt_connlimit.ko /lib/modules/2.6.18-164.el5/kernel/net/ipv4/netfilter/
为内核模块添加可执行权限
```
chmod +x /lib/modules/2.6.18-164.el5/kernel/net/ipv4/netfilter/ipt_connlimit.ko
depmod -a
modprobe ipt_connlimit
```
运行lsmod | grep x_tables出现如下提示,说明内核模块加载成功
```
x_tables               17349  7 ipt_connlimit,ipt_REJECT,xt_state,ip_tables,ip6t_REJECT,xt_tcpudp,ip6_tables
```

6.测试ipt_connlimit模块
```
iptables -I INPUT -p tcp --dport 22 -m connlimit --connlimit-above 5 -j REJECT

service iptables save
service iptables start
```

### 例子
* 限制同一IP同时最多100个http连接
```
iptables -I INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 -j REJECT
```
或
```
iptables -I INPUT -p tcp --syn --dport 80 -m connlimit ! --connlimit-above 100 -j ACCEPT
```
* 只允许每组C类IP同时100个http连接
```
iptables -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 --connlimit-mask 24 -j REJECT
```
* 只允许每个IP同时5个80端口转发,超过的丢弃
```
iptables -I FORWARD -p tcp --syn --dport 80 -m connlimit --connlimit-above 5 -j DROP
```
* 限制某IP最多同时100个http
```
iptables -A INPUT -s 222.222.222.222 -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 -j REJECT
```
* 限制每IP在一定的时间(比如60秒)内允许新建立最多100个http连接数
```
iptables -A INPUT -p tcp --dport 80 -m recent --name BAD_HTTP_ACCESS --update --seconds 60 --hitcount 100 -j REJECT
iptables -A INPUT -p tcp --dport 80 -m recent --name BAD_HTTP_ACCESS --set -j ACCEPT

iptables -A INPUT ! -s 127.0.0.1/32 -p tcp -m tcp --dport 8080 --tcp-flags FIN,SYN,RST,ACK SYN -m connlimit --connlimit-above 16 --connlimit-mask 32 --connlimit-saddr -j LOG --log-prefix "connlimit "
iptables -A INPUT ! -s 127.0.0.1/32 -p tcp -m tcp --dport 8080 --tcp-flags FIN,SYN,RST,ACK SYN -m connlimit --connlimit-above 16 --connlimit-mask 32 --connlimit-saddr -j REJECT --reject-with icmp-port-unreachable
iptables -A INPUT -p tcp -m tcp --dport 80 --tcp-flags FIN,SYN,RST,ACK SYN -m connlimit --connlimit-above 16 --connlimit-mask 32 --connlimit-saddr -j LOG --log-prefix "connlimit "
iptables -A INPUT -p tcp -m tcp --dport 80 --tcp-flags FIN,SYN,RST,ACK SYN -m connlimit --connlimit-above 16 --connlimit-mask 32 --connlimit-saddr -j REJECT --reject-with icmp-port-unreachable
```
