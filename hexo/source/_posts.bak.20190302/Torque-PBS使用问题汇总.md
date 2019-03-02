---
title: Toruqe PBS常见问题汇总
date: 2017-10-24 21:05:03
categories: 集群管理
tags: [Torque,PBS]
---

```
$ mpirun -np 25 python -c "print 'hey'"
--------------------------------------------------------------------------
There are not enough slots available in the system to satisfy the 25 slots 
that were requested by the application:
  python
Either request fewer slots for your application, or make more slots available
for use.
```
问题原因，如果使用了PBS提交任务，那么不应该直接指定np参数，应该使用PBS的变量来计算得到np参数的值

```
mpirun was unable to launch the specified application as it could not access
or execute an executable:
Executable: /orca_gtoint_mpi
Node: node01
while attempting to start process rank 0.
```
问题原因，exe变量有问题，就是说找不到可执行文件，检查mpirun后面的参数


```
[root@node272 ~]# qstat -q
socket_connect_unix failed: 15137
socket_connect_unix failed: 15137
socket_connect_unix failed: 15137
qstat: cannot connect to server (null) (errno=15137) could not connect to trqauthd
qstat: Error (15137 - could not connect to trqauthd)
```
问题原因：trqauthd服务没有启动，或者是该节点没有加入到Torque队列系统当中

解决办法：启动trqauthd服务，或者将节点加入到Torque队列系统中。
