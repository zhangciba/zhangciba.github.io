---
title: Linux IPMI 安装配置使用
date: 2016-05-26 11:07:15
categories: 集群管理
tags: ipmi
---
### 什么是IPMI? 百度百科的解释如下：
IPMI（智能平台管理接口）是一种开放标准的硬件管理接口规格，定义了嵌入式管理子系统进行通信的特定方法。IPMI 信息通过基板管理控制器 (BMC)（位于 IPMI 规格的硬件组件上）进行交流。使用低级硬件智能管理而不使用操作系统进行管理。
以上难以理解？你可以理解为通过这个接口可以：看到一些服务器硬件信息、实现远程开关机、远程重启服务器。应用场景如：

<!-- more -->
* 服务器宕机，这时候通过SSH已经无法远程连接，服务器又托管在IDC，你又打电话苦寻网管员无果，可以通过IPMI来进行远程重启。
*  集群服务，如RHCS中的内部Fence设备。
目前服务器基本上都集成了这个接口，可能各个服务器配置不同，所以如果没有意外，可以在服务器上架的时候配置就一下IPMI，为以后操作带来方便。
目前DELL R710 R910 系列服务器的IPMI，集成在第一块网卡eth0，你需要将网线连接第一块网卡eth0到交换机。eth0网卡启动与否并不影响它的使用。所以服务器的IP地址则推荐选择其他的网卡。

### IPMI配置途径：
* 通过开机的BIOS配置，网上图文教程比较多，即开机ctrl+E进入配置界面。
* 主要用于通过指令来配置，适用于服务器已经上架，IDC机房距离又较远，实在懒得跑过去一趟。前提是第一块网卡得连上线，不然没办法测试。

### CentOS 上的配置方法：
#### 安装相关组件，主要是OpenIPMI，并启动服务：
```
yum install OpenIPMI OpenIPMI-devel OpenIPMI-tools OpenIPMI-libs
/etc/init.d/ipmi start
chkconfig ipmi on
```

#### 进行IPMI的基本网络配置：
网上很多教程都有-I open参数，其实这个参数是默认的。不要统统都抄过来啊。
以下指令分别配置了IP地址、掩码、网关、允许进入开关。IP地址最好与服务器IP在同一网段。
```
ipmitool lan set 1 ipaddr 192.168.1.70
ipmitool lan set 1 netmask 255.255.255.0
ipmitool lan set 1 defgw ipaddr 192.168.1.1
ipmitool lan set 1 access on
ipmitool lan print 1 # 检查网络配置结果
```

#### 开启默认用户、设置默认密码：
```
ipmitool lan set 1 user
ipmitool lan set 1 password 123123
ipmitool user list 1 # 显示当前用户列表
```

#### 通过查看用户列表。可以看到当前有两个用户，一个是默认匿名用户，一个是root。而root的uid = 2。
所以要设置一下root用户的密码，按照提示输入两次密码：
```
ipmitool user set password 2
```
#### 在多台服务器上配置好IPMI后，测试可以ping通设置好的IP地址。
以下为两种检验方法：
ping 192.168.1.70
ipmitool -H 192.168.1.70 -U root power status
正常返回结果会是：power is on。

#### 好了，你可以开关机与重启的测试：
```
ipmitool -H 192.168.1.70 -U root power on
ipmitool -H 192.168.1.70 -U root power off
ipmitool -H 192.168.1.70 -U root power reset
```
RHCS中的Fence配置方法：
在做RHCS集群中，选择IPMI进行Fence配置时，仅仅验证ipmitool测试正常是不够的。还需要验证RHCS中的agent是否可以正常工作，因为我通过ipmitool lan print 1 发现验证仅支持MD5，所以使用以下指令进行agent的验证试探。
```
fence_ipmilan -v -a 192.168.1.70 -l root -p 123123 -o status -A md5
```

以上参数分别表示IP地址、用户名、密码、验证方法。
验证通过后，RHCS的配置文件中也要加上验证方法的配置：
```
auth="md5" ipaddr="192.168.1.70" login="root" name="CMS01" passwd="123123"/>

```
这样才能确保集群Fence正常。
网上一堆案例都搞不清auth的问题，有贴auth="none"的，有贴auth="password"的，只有通过上述方法验证后你才能确定到底是什么原因？
最后说一句。IPMI在RHCS中属于内部Fence设备，如果你拔掉服务器电源线，它是没办法正常工作的。

不过服务器电源一般都不是单电，可以不用考虑这种情况。

