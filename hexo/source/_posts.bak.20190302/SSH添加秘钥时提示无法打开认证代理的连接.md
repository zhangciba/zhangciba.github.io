---
title: SSH添加秘钥时提示无法打开认证代理的连接
date: 2016-04-11 20:52:02
tags: SSH
categories: ssh
---

我们在配置SSH客户端免密码登录时，需要在登录节点的SSH客户端中添加RSA秘钥才可以免密码登录到另外一台服务器。但是我们在使用ssh-add命令添加时，会遇到这个错误。
```
could not open a connection to your authentication agent
```
这是时候可以执行 
```
eval 'ssh-agent -s'
```
查看ssh-agent是否启动了，如果启动了，一般是不会报错的。再执行一次'ssh-add ./.ssh/id_rsa'命令，如果还是报错，则执行以下命令：
```
exec ssh-agent bash
```
<!-- more -->
这样问题就可以解决了。

但是这样每次关闭终端之后或者重启之后，都需要重新添加秘钥才行，因为之前的添加是临时的，可以将添加秘钥的命令写入到系统配置文件中Ubuntu的.profile或者CentOS的/etc/profile文件中。这样系统开机之后，就自动为我们将秘钥添加到ssh-agent中去了。省去了每次都需要添加的麻烦。

注：如果是在Windows或者MacOSX中，大部分的SSH工具(Secure Shell Client、SecureCRT、Putty)等都可以直接在配置中添加id_rsa秘钥，如果是Mac OSX系统，则可以在钥匙串中添加该秘钥，这样就可以不用每次都需要进行秘钥添加的操作了。

