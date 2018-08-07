---
title: undefined reference to pthread_create
date: 2017-11-27 07:38:38
categories: Linux开发
tags: [pthread]
---

在使用pthread库进行线程开发的时候，如果包含了pthread.h头文件后，在编译代码的时候还是出现了如下错误。可能是没有在编译时没有链接相关的函数库。
```
undefined reference to `pthread_create'
collect2: ld returned 1 exit status
make: *** [threadid] Error 1
```
在编译时，加入-l pthread选项即可。
如果是用Makefile，也是一样的
```
CC = gcc
CFLAGS = -I/home/cat/apue/apue.2e/include -Wall -g、
threadid: threadid.o
$(CC) $(CFLAGS) -o $@ $^ -lpthread
```
