---
title: Evaluating Container Platforms at Scale
date: 2016-06-23 08:58:45
categories: 云计算
tags: 
 - Container-Platforms
---

[原文链接请点我:Evaluating Container Platforms at Scale](https://medium.com/on-docker/evaluating-container-platforms-at-scale-5e7b44d93f2c#.30uszmvzv)

作者发现查找比较Swarm和Kubernetes性能的操作很难，最后发现，*容器启动延迟和容器列出时间*是最好的用于评估的尺度。

实时系统和实时服务对于容器的启动的延时要求很高。
列出所有容器对于集群管理员的用户体验来说关联最大。

Kubernetes和Swarm都提供了一个API来获取容器列表的功能。
