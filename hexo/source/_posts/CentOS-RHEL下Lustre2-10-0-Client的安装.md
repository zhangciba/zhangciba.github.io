---
title: CentOS/RHEL下Lustre2.10.0 Client的安装
date: 2017-11-14 16:39:04
categories: 集群管理
tags: [Lustre, FS, 集群管理]
---

# Lustre2.10.0 Client客户端安装-CentOS/RHEL

学校超算中心使用了Lustre文件系统，速度超级快，不是一般的文件系统可以比的，Top500的超级计算机中有50%以上使用了该文件系统。该文件系统也是命途多舛，几经波折最后被Intel老大收购了。目前比较新的版本的2.10.1这个版本。支持最新的RHEL/CentOS7.3。

由于超算中心的系统是基于RHEL6.2的，比较老了，而很多HPC软件的新特性都是基于RHEL7.0来设计的，为了使用HPC软件新特性，因此安装了新的系统，由于Lustre的安装过程比较复杂，还需要编译内核，加载模块，所以一直都不敢尝试。

但是无奈，如果不安装Lustre，并行文件系统没法用，计算速度大打折扣。因此还是硬着头皮安装了。由于目前网上有关的Lustre资料比较少，所以就分享一下自己装Lustre客户端的经验给大家了。其实新版本的安装过程简单了不少，以前的还需要重新编译内核，要复杂很多倍的。

- **下载client端相关的RPM包**
到这里[Intel lustre官网](https://downloads.hpdd.intel.com/public/lustre/)下载相关版本的lustre包，由于服务器端已经部署好了，不敢随意乱动，所以只安装了计算节点的Lustre client端，即客户端。
- **yum install kernel-devel kernel-headers **
安装编译软件需要的一些基本工具包，如果还差，可以一起安装好了，免得影响Lustre的编译。
- **rpm -ivh 安装相关的包**
实际上，可以直接安装网上下载的包，因为针对常见的CentOS版本，Intel已经帮我们编译好了。安装的时候，一定要注意了软件版本和内核版本的一致，否则无法正常安装。
举例说明，如果你在官网下载的路径是
https://downloads.hpdd.intel.com/public/lustre/lustre-2.10.0/el7/client/RPMS/x86_64/

请注意，可以直接下载RPM目录下面的包即可，SRPMS是源文件的RPM包，主要针对官网没有编译好的系统版本，可能需要自己去编译。

kmod-lustre-client-2.10.0-1.el7.x86_64.rpm
kmod-lustre-client-tests-2.10.0-1.el7.x86_64.rpm
lustre-client-2.10.0-1.el7.x86_64.rpm
lustre-client-debuginfo-2.10.0-1.el7.x86_64.rpm
lustre-client-dkms-2.10.0-1.el7.noarch.rpm
lustre-client-tests-2.10.0-1.el7.x86_64.rpm
lustre-iokit-2.10.0-1.el7.x86_64.rpm

将以上包下载到需要挂载Lustre文件系统的服务器上，然后陆续开始安装这些包。如果你的机器联网了，那么可以使用yum install packagename来进行安装，因为这样可以帮你自动解决一些未安装的依赖包的问题。如果没有联网，那么老实使用rpm -ivh packagename的形式安装。

安装也是有顺序的，大致的顺序如下：
kmod-lustre-client-2.10.0-1.el7.x86_64.rpm
kmod-lustre-client-tests-2.10.0-1.el7.x86_64.rpm
lustre-iokit-2.10.0-1.el7.x86_64.rpm
lustre-client-dkms-2.10.0-1.el7.noarch.rpm
lustre-client-2.10.0-1.el7.x86_64.rpm
lustre-client-debuginfo-2.10.0-1.el7.x86_64.rpm
lustre-client-tests-2.10.0-1.el7.x86_64.rpm

- **depmod -a**
- **modprobe lustre加载lustre模块**
```
modprobe lustre
```
加载Lustre文件系统有关模块。
如果是在以前，还需要在/etc/modprobe.d/目录下面添加一个lustre.conf文件，该文件配置了Lustre使用的网络信息。新版本的Lustre已经不需要了，文件系统客户端自动识别和配置了。但是还是贴出来给大家看看。

```
options lnet networks="tcp0(eth0)"
```
如果你的网卡或者网桥的名字不是eth0，需要修改为网卡或网桥的名字，然后再执行modprobe lustre命令

- **挂载文件系统**
```
mount -t lustre "lsserver@tcp0:/sharefs" /lustre
```
其中lsserver是Lustre服务器端的主机名，根据实际情况修改。
sharefs是Lustre服务器端提供的文件系统名称
/lustre是在客户端创建的挂载点。

- **开始使用**
正常挂载后，就可以像本地文件系统一样使用Lustre了。


