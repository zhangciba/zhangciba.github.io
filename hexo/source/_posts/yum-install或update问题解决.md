---
title: yum install或update问题解决
date: 2016-04-25 15:25:06
tags: yum
categories: Linux
---

当我们在使用yum进行软件安装或者软件更新时，经常会遇到这个错误。
```
Cannot find a valid baseurl for repo: base/7/x86_64
```

如果提示这个错误，先检查一下网络是不是通的，可以ping测试一下能够连接到国内的网络。
```
ping www.baidu.com
```
<!-- more -->

如果可以连通，可以再试试国外的网站，
```
ping www.github.com
```

如果也可以连通国外的网站那么执行下面的命令重新建立repo缓存。
```
yum clean all #清除缓存
yum makecache #建立缓存
```


