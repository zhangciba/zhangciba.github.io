---
title: ipmitool使用总结
date: 2016-05-25 16:02:55
categories: 集群管理
tags: ipmitool
---

需要加载的模块
ipmi_watchdog          17414  0
ipmi_poweroff           8532  0
ipmi_devintf            8049  0
ipmi_si                42401  2
ipmi_msghandler        35992  4 ipmi_watchdog,ipmi_poweroff,ipmi_devintf,ipmi_si

<!-- more -->
需要启动的服务
service ipmi start

查看网络状态
ipmitool lan print
查看电源状态
ipmitool -H 172.10.10.1 -U admin -P admin chassis power status;


参考资料的链接
[ipmitool常见问题解决方法链接][linka]
[ipmitool排错记录][linkb]
[ipmi操作指南][linkc]


[linka]:http://blog.csdn.net/c9h8o4/article/details/17138029
[linkb]:http://blog.sina.com.cn/s/blog_702eef650100y4ar.html
[linkc]:http://blog.csdn.net/yunsongice/article/details/5408802
