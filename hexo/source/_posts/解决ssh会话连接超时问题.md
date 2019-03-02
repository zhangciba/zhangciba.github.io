---
title: 解决ssh会话连接超时问题
date: 2016-04-25 09:30:52
tags: ssh连接超时
categories: ssh
---

ssh连接超时问题解决方案：
1.修改server端的etc/ssh/sshd_config
ClientAliveInterval 60 ＃server每隔60秒发送一次请求给client，然后client响应，从而保持连接
ClientAliveCountMax 3 ＃server发出请求后，客户端没有响应得次数达到3，就自动断开连接，正常情况下，client不会不响应
<!-- more -->
2.修改client端的etc/ssh/ssh_config添加以下：（在没有权限改server配置的情形下）
ServerAliveInterval 60 ＃client每隔60秒发送一次请求给server，然后server响应，从而保持连接
ServerAliveCountMax 3  ＃client发出请求后，服务器端没有响应得次数达到3，就自动断开连接，正常情况下，server不会不响应

3.另一种方式： 
不修改配置文件
在命令参数里ssh -o ServerAliveInterval=60 这样子只会在需要的连接中保持持久连接， 毕竟不是所有连接都要保持持久的

