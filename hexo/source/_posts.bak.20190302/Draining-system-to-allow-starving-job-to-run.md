---
title: Draining system to allow starving job to run
date: 2017-11-16 15:17:40
categories: 集群管理
tags: [Torque,PBS,Linux,Cluster]
---

```
Draining system to allow starving job to run
```
意思是，队列中还有排队非常久的饥饿进程，为了更公平合理使用队列，所以将新提交的作业做置为排队状态以让出资源运行排队的饥饿作业。

