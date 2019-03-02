---
title: Kubernetes搭建Docker集群(原创)
date: 2016-04-14 20:42:33
tags: Kubernetes,Docker
categories: 云计算
---

&#160; &#160;这几天因为实验需要，准备搭建一个Kubernetes搭建一个由10台物理机组成的Docker集群来进行HPC运算并测试基于Docker的操作系统级虚拟化技术对于HPC运算能力的影响。
&#160; &#160;在网上百度了一下，基本上不到5篇文章，这些文章都有一个特点，有国内较早的Docker布道者刘天斯写的文章，由于文章写的比较早，整个Kubernetes变化比较大，安装方法本来就不是很详细，另外版本变化太大，已经不适应现在的安装情况了。还有一些版本比较新的安装方法，但是都没有讲解的比较详细，本人一直都觉得国内写的博文和教程不够全面，就是没有把一些可能出现的问题，以及安装的途径以及方法写的比较全。
&#160; &#160;我也是看了这些文章很久，至今没有一篇文章可以把集群搭建起来。于是去看官方文档，官方文档更加简单，根本没法安装要求来搭建自己的集群。
 &#160; &#160;无赖，只有求救自己手上国内所有的Docker书籍，大概有5本左右。发现也是一笔带过，而且基本都是抄的网上的教程。已醉。
&#160; &#160;所以只有自己写啦。集群已经搭建起来了，这中间遇到了很多问题，暂时还没有时间写出来，不过本文会陆续更新，力求给大家呈现一个原理与实践兼具，而且将其中可能会遇到的问题和大家一起进行分析与探讨，让大家能够通过这个教程就可以大致了解Kubernetes的特点以及Docer容器的特点，为以后的进一步使用和研究打基础。
<!-- more -->
搭建过程如下所示：
### 搭建环境
9台联想启天普通PC
硬件配置如下：
Intel Core Pentium E5300 Daul CPU
2GB DDR2 内存
300GB SATA硬盘
1000M Ethernet网卡

操作系统是CentOS 7.0
内核版本是

### 机器规划
1台机器作为Kubertetes的Master节点，其余8台为Slave节点。
机器的相关属性如下：
```
========================================================================
| 节点名称    |    IP地址     |   主机名 |      安装的软件              |
| master node | 211.69.197.49 | docker09 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.41 | docker01 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.42 | docker02 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.43 | docker03 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.44 | docker04 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.45 | docker05 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.46 | docker06 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.47 | docker07 | Docker、Kubernetes、Flannel	|
| minion node | 211.69.197.48 | docker08 | Docker、Kubernetes、Flannel	|
========================================================================
```
### master节点进行安装


附录：网上写得比较好的安装Kubernetes集群的文章
1.Creating a kubernetes cluster to run docker formatted container images
	 https://access.redhat.com/documentation/en/red-hat-enterprise-linux-atomic-host/7/getting-started-with-containers/chapter-4-creating-a-kubernetes-cluster-to-run-docker-formatted-container-images
 
2.Kubernetes 1.01 the build
	http://www.dasblinkenlichten.com/kubernetes-101-the-build/

