---
title: 'error while loading shared libraries: libstdc++.so.6: wrong ELF class: ELFCLASS64'
date: 2017-06-02 16:38:44
categories: 高性能计算
tags: [gcc,ld,libstd]
---

Linux服务器执行一些命令时，经常会出现一下错误：
```
error while loading shared libraries: libstdc++.so.6: wrong ELF class: ELFCLASS64
```
错误原因是：64位的程序尝试读取32位的标准库函数，导致找不到。因为Linux服务器上面没有安装libstdc++.i686即32位C++标准库函数或者是库的链接配置错误。

解决办法，安装libstdc++.i686，或者修改库链接。
<!--more-->
```
#CentOS RHEL
yum install libstdc++.i686
#Ubuntu
apt-get install libstdc++.i686
```

```
rm -f /usr/liblibstdc++.so.6
ln -s /usr/lib/libstdc++.so.6.0.8 /usr/lib/libstdc++.so.6
```
