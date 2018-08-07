---
title: 'shmget:permission denied'
date: 2017-05-24 09:39:42
categories: Linux开发
tags: [Shared Memory,共享内存]
---


shm_addr=(char*)shmat(shm_id,NULL,0); 返回-1。

perror 打印出：permission denied 

shmget的时候要加上0666权限，比如：

shm_id=shmget(key,4096,IPC_CREAT|0666);

但是还是不行。

后来想到ftok的时候用到了一个filepath

key=ftok(filepath,0);

这个ftok必须是已存在真实文件，是不是文件的权限问题？

用chmod 777 filepath之后，发现还是不行

最后采用sudo运行程序可以。

