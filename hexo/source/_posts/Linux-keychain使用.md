---
title: Linux keychain使用
date: 2016-05-13 20:53:37
categories: ssh 
tags: keychain
toc: true
---
一篇不错的讲解Linux ssh-agent和keychain的文章，很实用。
http://www.linuxidc.com/Linux/2011-08/39872p2.htm
http://bbs.chinaunix.net/thread-2197964-1-1.html

```
/usr/bin/keychain --clear ~/.ssh/keyOf191 ##开启一个新的shell的时候提示输入密码，提高安全性
~/.keychain/$HOSTNAME-sh > /dev/null  ##开启一个新的shel启用keychain
```
