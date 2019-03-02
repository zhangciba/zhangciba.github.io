---
title: CentOS7修改内核启动默认顺序
date: 2017-11-15 10:02:44
categories: Linux
tags: [grub2,启动顺序,CentOS7,Linux]
---

### 查看系统启动菜单
我们知道，centos 6.x是通过/etc/grub.conf就行内核启动顺序修改的，而且比较直观查看。但centos 7的系统和6就不一样了，是通过grub2为引导程序。下边简单说下centos 7的内核启动顺序如何修改.
```
[root@node301 ~]# cat /boot/grub2/grub.cfg |grep menuentry
 
if [ x"${feature_menuentry_id}" = xy ]; then
  menuentry_id_option="--id"
    menuentry_id_option=""
    export menuentry_id_option
    menuentry 'CentOS Linux (3.10.0-327.22.2.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-327.el7.x86_64-advanced-80b9b662-0a1d-4e84-b07b-c1bf19e72d97' {
	    menuentry 'CentOS Linux (3.10.0-327.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-327.el7.x86_64-advanced-80b9b662-0a1d-4e84-b07b-c1bf19e72d97' {
		    menuentry 'CentOS Linux (0-rescue-7d26c16f128042a684ea474c9e2c240f) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-7d26c16f128042a684ea474c9e2c240f-advanced-80b9b662-0a1d-4e84-b07b-c1bf19e72d97' {
```

### 设置默认的启动内核

比如我们选择上边中的CentOS Linux (3.10.0-327.el7.x86_64) 7 (Core)这个内核为默认启动。
```
[root@node301 ~]# grub2-set-default "CentOS Linux (3.10.0-327.el7.x86_64) 7 (Core)" 
#配置默认内核
```
验证是否修改成功：

```
[root@node301 ~]# grub2-editenv list 
saved_entry=CentOS Linux (3.10.0-327.el7.x86_64) 7 (Core)
```

### 重启机器
```
sync
sync
reboot
```

机器重启后，查看内核版本是否正确。
```
[root@node301 ~]# uname -r
3.10.0-327.el7.x86_64
```
