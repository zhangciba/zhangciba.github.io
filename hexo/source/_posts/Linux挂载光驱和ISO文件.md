---
title: Linux挂载光驱和ISO文件
date: 2016-05-21 10:22:04
categories: Linux
tags: 挂载
---

## 挂载光驱
linux的硬件设备在/dev目录下，光驱也是其中。
/dev/cdrom表示光驱，挂载光驱的方法如下(以root身份)：
```
#创建挂载目录
mkdir /mnt/cdrom
#执行挂载操作
mount  -t auto  -o ro  /dev/cdrom    /mnt/cdrom  #不加参数也能自动挂上。
#-t auto类型自动， -o ro只读模式
```

## 挂载ISO文件
对于iso镜像文件可以进行挂载
```
#创建挂载目录
mkdir -p /mnt/iso
#挂载ISO文件
mount -t iso9660  -o loop  iso文件   /mnt/iso
```
