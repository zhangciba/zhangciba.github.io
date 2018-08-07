---
title: CentOS设置root用户邮箱
date: 2018-05-18 19:48:04
categories: Linux
tags: root邮箱
keywords: 邮箱
description: CentOS设置root用户邮箱
---

设置root邮箱
   在系统出现错误或有重要通知发送邮件给root的时候，让系统自动转送到通常使用的邮箱中，这样方便查阅相关报告和日志。
```
[root@node999 root]# vi /etc/aliases
root: bestmylover@hotmail.com　← 加入自己的邮箱地址
[root@node999 root]# newaliases　← 重建aliasesdb
/etc/aliases: 79 aliases, longest 23 bytes, 829 bytes total
[root@node999 root]# echo test | mail root　← 发送测试邮件给root
```
