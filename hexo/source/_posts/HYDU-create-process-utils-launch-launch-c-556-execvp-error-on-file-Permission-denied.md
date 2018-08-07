---
title: 'HYDU_create_process (./utils/launch/launch.c:556): execvp error on file * (Permission denied)'
date: 2017-05-24 23:24:37
categories: 高性能计算
tags: [并行计算,MPI,高性能计算]
---

HYDU-create-process-utils-launch-launch-c-556-execvp-error-on-file-Permission-denied

使用MPICH2.0版本或者OpenMPI1.x版本，这些版本的MPI在运行MPI程序之间，需要运行一下mpdboot命令。
```
mpdboot -f hostfile
#将其中的hostfile换为节点列表文件的全路径。例如说/home/jim/hostfile
```
如果不报错，然后在运行自己的MPI程序
```
mpirun -np 16 -hostfile /path/to/hostfile /path/to/my/mpi/program
```

