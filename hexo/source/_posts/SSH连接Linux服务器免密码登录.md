---
title: SSH连接Linux服务器免密码登录
date: 2016-04-11 10:03:40
tags: SSH
categories: ssh
---

我们平时在使用Linux服务器时，一般都会采用SSH方式进行远程连接。这种方式给我们带来了很多便利。但是由于经常要输入密码，所以有点麻烦，采用RSA秘钥的方式进行配置，以后登录就只需要提供RSA私钥就可以了，不同再次输入密码。是不是很方便。

那么下面我们来说说具体的配置吧。例如，我们要使用电脑A(Windows或者Mac OSX或Linux系统)的SSH工具(Secure Shell Client、SecureCRT、Pitty、iTerm等)登录到服务器B，那么我们需要做哪些工作呢。
<!-- more -->
# 1.使用sshd-keygen命令生成密钥对
我们可以在任何一台机Linux服务器上生成，例如，我们就是在服务器B上面进行生成。具体命令是
```
ssh-keygen -t rsa
```
回车之后，首先会让你输入文件名，然后提示你输入密钥的口令。之后就会在~/.ssh/ 目录下生成两个文件，一般默认文件名为id_rsa和id_rsa.pub，id_rsa是秘钥，id_rsa.pub是公钥。
id_rsa.pub需要放在服务器B上面，而id_rsa需要放在我们登录时使用的机器上面，也就是说，id_rsa就是我们平时要随身携带在U盘中的。
<!-- more -->

# 2.将id_rsa.pub中的公钥加入到authorized_keys中
使用如下命令，
```
cd ~/.ssh/ #进入到id_rsa.pub所在的目录
cat id_rsa.pub >>authorized_keys
```
# 3.修改authorized_keys文件的权限
让本用户组之外的其他用户都不可以读取和修改
```
sudo chmod 600 authorized-keys
```

# 4.配置SSH，允许使用RSA秘钥登录
修改ssh的配置文件，配置文件是/etc/ssh/sshd_config，将RSAAuthentication和
PubkeyAuthentication两个选项的前面的'#'号去掉，也就是允许使用秘钥或者公钥进行登录，如果想同时禁止使用密码登录，那么可以将PasswordAuthentication yes修改为PasswordAuthentication no就可以了，以后就可以只使用公钥和秘钥登录了。
```
#将这两个选项前面的#去掉，
#RSAAuthentication yes
#PubkeyAuthentication yes
这样子就是的啦
RSAAuthentication yes
PubkeyAuthentication yes
```
```
#希望以后可以使用密码登录，那么可以保持这个选项不变
PasswordAuthentication yes
#希望以后不可以使用密码登录，将这个选项修改为no
PasswordAuthentication no
```

# 5.重启SSH服务
```
service ssd restart
#重启SSH服务
```

# 6.将id_rsa文件拷贝到需要登录到服务器B的机器上面
可以使用scp命令，或者直接从服务器拷贝等方式将id_rsa文件复制到需要登录到服务器B的机器上面，在使用SSH工具的时候，在选项里面添加id_rsa这个秘钥，登录的时候就不需要输入密码了。


