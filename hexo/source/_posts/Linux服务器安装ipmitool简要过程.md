---
title: Linux服务器安装ipmitool简要过程
date: 2016-06-06 12:52:04
categories: 集群管理
tags: ipmitool
---
#### 下载ipmitool：http://ipmitool.sourceforge.net/
#### 确定gcc工具已经安装好
<!-- more -->
#### 在Linux系统上加载启用IPMI驱动：
```
insmod /lib/modules/2.6.32-220.el6.x86_64/kernel/drivers/char/ipmi/ipmi_msghandler.ko
insmod /lib/modules/2.6.32-220.el6.x86_64/kernel/drivers/char/ipmi/ipmi_devintf.ko
insmod /lib/modules/2.6.32-220.el6.x86_64/kernel/drivers/char/ipmi/ipmi_si.ko

```
#### 检查你的/dev目录下出现了ipmi0这个设备：
```
ls -l /dev/ipmi*
```

#### 解压缩ipmitool-1.8.11.tar.gz
```
tar zxvf ipmitool-1.8.11.tar.gz
cd ipmitool-1.8.11
```

#### 开始安装ipmitool：
```
./configure
make
make install
```

#### ipmitool命令将被安装到/usr/local/bin/ipmitool

#### 现在你就可以用了
