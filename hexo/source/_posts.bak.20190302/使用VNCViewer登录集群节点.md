---
title: 使用VNCViewer登录集群节点
date: 2016-06-29 17:50:30
categories: 集群管理
tags: VNCViewer
---

有时候连接服务器时需要使用图形界面，VNC恰好给我们提供了这样的服务，闲话少说，直接进入正题。
整体过程如下：
<!--more-->
### SSH登录到管理节点
```
#管理节点node307
ssh root@202.114.10.172

#另外一个节点node308
ssh root@202.111.10.171
```

### 设置本账户的VNC密码
```
vncpasswd
#使用这个命令设置VNC登录密码
```

### 开启一个VNCServer服务
```
vncserver -autokill
#开启一个vncserver服务
#它会返回一个端口号形式为 node307:7
```

### 使用VNCViewer登录到集群
VNCViewer客户端官方下载地址为:
[点我下载VNCViewer](https://www.realvnc.com/download/viewer/)

然后输入刚才在服务器端使用vncserver命令开启的服务端口返回的端口号，node307:7，点击连接之后，输入密码即可。

最后就可以进入到集群了。

### VNC登录到自己的节点
方法与登录到集群管理节点一样

#### 在自己的节点安装VNCServer服务软件。
参考网上链接

#### 设置VNC密码
```
vncpasswd
```

#### 开启VNCServer服务
```
vncserver
```
#### 使用VNCViewer连接服务器
