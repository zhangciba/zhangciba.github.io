---
title: Intra-Node Communication Mechanisms
date: 2017-11-06 08:51:47
categories: 论文阅读
tags: [进程通信,同主机进程通信]
---

##### NIC-Level Loopback
Two DMA operations
##### User-space Shared Memory
The sending process copies the message to the shared memory area.
The receiving process then copy over the message to its own buffer.
##### Kernel based Shared Memory

* The receiving process needs to bring the sending process' buffer into its cache
* And the receiving process can write this buffer into its own receive buffer

![Intra Node Communication Shemes](http://7xsnoh.com1.z0.glb.clouddn.com/Intra-Node%20Communication%20Schemes.png)
