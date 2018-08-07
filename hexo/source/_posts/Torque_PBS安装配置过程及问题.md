---
title: Torque PBS安装配置过程及问题
date: 2017-05-09 21:22:59
categories: 集群管理
tags: [Torque, PBS]
---

CSDN博客上面的[安装参考教程](http://blog.csdn.net/dream_angel_z/article/details/44225669/)

详细的拍错文档，值得参考[Torque Troubleshooting文档](http://www.silas.net.br/doc.notes/hpc/torque.html)

## 安装步骤简单归纳如下

### 编译安装
1.集群/etc/hosts文件配置，将集群每个主机的名称和IP地址信息写入到/etc/hosts文件，并将该文件拷贝到每个节点上面，包括PBS服务器节点和PBS计算节点。
<!--more-->

2.配置集群所有节点之间的SSH免秘钥访问，最简单的方法就是在某一台机器生成了SSH密钥文件之后，统一将PBS用户主目录下的.ssh目录拷贝到所有集群其他节点对应的目录下面。实际上，使用PBS队列的集群一般都使用了NFS或者其他分布式文件系统来挂载/home目录，即，所有节点的/home目录信息是一致的，由NFS或其他分布式网络文件系统来提供，所以SSH免密钥登录配置是标准配置，一般集群都提供好了，如果没有提供，请自行配置，方法如前所述。

3.下载Torque源码，并解压到当前目录下面。

4.进入Torque源码目录，执行`./configure --prefix=/opt/troque`命令，如果编译过程中出现错误，根据错误提示解决问题即可，一般情况下都是某一种编译器缺失或者编译器不完整等问题，安装相关软件或者解决版本冲突即可。

5.编译完成后，执行`make & make install` 命令，编译并安装。

6.安装完成后，执行`make packages`生成安装脚本。

其中，3，4，5，6这四步是在PBS服务节点上面进行的。

下面进行PBS服务节点和计算节点的安装配置工作。

### PBS服务器节点配置
S1.在上面的步骤5执行完成之后就已经安装好了。现在需要做的是配置工作。

S2.首先将Torque源代码目录下$TORQUE_SOURCE_DIR/contrib/init.d/目录下面将pbs_server,pbs_sched.pbs_mom,trqauthd等四个文件拷贝到/etc/init.d/目录下面，其实就是将这四个服务器的脚本拷贝到系统服务脚本目录。然后执行下面的命令添加服务，并启动服务。
	```
	for i in {pbs_server,pbs_sched,trqauthd,pbs_mom}
	do
		chkconfig --add $i;
		chkconfig $i on;
		service $i start;
	done
	```

好的，执行完命令之后，PBS服务器端的四个服务都已经启动了。

S3.PBS各服务进程介绍
pbs_server主要负责接收计算节点的作业提交，位于服务节点
pbs_sched主要负责作业调度，位于服务节点
pbs_mom负责监控本机并执行作业，位于所有节点
trqauthd负责qsub、qdel、qstat命令执行时，连接到pbs_server的授权，位于所有节点

S4.配置计算节点，默认torque安装目录是在/var/spool/torque。进入到/var/spool/torque/server_priv目录，编辑nodes文件，添加计算节点。
```
	node1 np=16
	node2 np=16
  #以此类推，格式如下
  #计算节点主机名  np=处理器个数
```
配置完成后，执行`service pbs_server restart`命令重启pbs_server服务，执行pbsnodes既可以看到节点信息。当然，这里还没有配置好计算节点，所以暂时没有显示任何信息。

S5.配置系统环境变量
编辑profile文件
```
vim /etc/profile
```
加入如下内容
```
TORQUE=/opt/torque
MAUI=/opt/maui
if [ "`id -u`" -eq 0 ];
then
    PATH=$PATH:$TORQUE/bin:$TORQUE/sbin:$TORQUE/bin:$MAUI/sbin:$MAUI/bin
else
    PATH=$PATH:$TORQUE/bin:$MAUI/bin
fi
```

```
source /etc/profile
```

S5.服务节点基本配置完成，队列等配置后续再说。接下来安装和配置计算节点。

### PBS计算节点安装配置
C1.将服务器端编译过的源代码中的torque-package-{clients,mom}-linux-x86_64.sh两个文件拷贝到计算节点/root目录，并且将源码目录下contrib/init.d/目录的pbs_mom、trqauthd两个文件拷贝到所有计算节点的/etc/init.d/目录，好的上面所述的两种文件都拷贝好了之后开始安装工作。

C2.在每个计算节点的/root目录下面执行下面两个命令，安装PBS的客户端程序。
	```
	./torque-package-clients-linux-x86_64.sh --install
	./torque-package-mom-linux-x86_64.sh --install
	```
执行完上面的命令之后，PBS计算节点就安装好了。接下来进行PBS计算节点的配置工作。

C3.进入/var/spool/torque/目录，查看server_name文件，查看里面的内容是不是PBS服务节点的名称，如果不是，则修改为PBS服务节点的主机名。例如说PBS主机名为master，那么server_name中的内容为
	```
	master
	```
接着进入到/var/spool/torque/mom_priv/目录，配置config文件，如果没有则创建，内容如下：
```
$pbsserver      master            # note: hostname running pbs_server
$logevent       255               # bitmap of which events to log
```
如果已经存在，则修改master修改为你的PBS服务节点的主机名，如果不存在这个文件，那么按照上面的内容创建一个这样的文件。

现在服务基本上已经配置好了。现在可以启动PBS计算节点的服务器了。

C4.执行下面的命令配置PBS计算节点的服务。

```
for i in {trqauthd,pbs_mom}
     do
         chkconfig --add $i;
         chkconfig $i on;
         service $i start;
     done
```

C5.配置系统环境变量
编辑profile文件
```
vim /etc/profile
```
加入如下内容
```
TORQUE=/opt/torque
MAUI=/opt/maui
if [ "`id -u`" -eq 0 ];
then
    PATH=$PATH:$TORQUE/bin:$TORQUE/sbin:$TORQUE/bin:$MAUI/sbin:$MAUI/bin
else
    PATH=$PATH:$TORQUE/bin:$MAUI/bin
fi
```

```
source /etc/profile
```
如果不进行环境变量配置，那么执行命令时会提示该命令不存在的错误。


### 常见的问题及可能的解决办法

```
socket_connect_unix failed: 15137
qnodes: cannot connect to server node308, error=15137 (could not connect to trqauthd)
```
遇到以上问题，一般的原因有以下两种：
1.管理节点和计算节点的trqauthd没有启动；
2./var/spool/torque/mom_priv中的config文件中的pbsserver参数设置有问题。
3.防火墙没有关闭。

解决办法分别如下：
1.重启管理节点和所有计算节点的trqauthd服务，service trqauthd restart；
2.将config文件中的$pbsserver参数设置为正确的管理节点主机名；
3.将防火墙iptables或者firewalld等关闭，或者添加端口允许。



### Torque队列配置

```
qmgr -c 'set server operators += $USERNAME'
qmgr -c 'set server managers += $USERNAME'
qmgr -c 'set server scheduling = true'
qmgr -c 'set server keep_completed = 300'
qmgr -c 'set server mom_job_sync = true'
qmgr -c 'create queue batch'
qmgr -c 'set queue batch queue_type = execution'
qmgr -c 'set queue batch started = true'
qmgr -c 'set queue batch enabled = true'
qmgr -c 'set queue batch resources_default.walltime = 1:00:00'
qmgr -c 'set queue batch resources_default.nodes = 1'
qmgr -c 'set server default_queue = batch'
qmgr -c 'p s' 
```


