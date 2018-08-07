---
title: 'ipmitool启动报错-Could not open device at /dev/ipmi0 or /dev/ipmi/0 or /dev/ipmidev/0: No'
date: 2016-05-25 11:05:51
categories: 集群管理
tags: ipmitool
---

modprobe ipmi_watchdog
modprobe ipmi_poweroff
modprobe ipmi_devintf
modprobe ipmi_si
modprobe ipmi_msghandler

