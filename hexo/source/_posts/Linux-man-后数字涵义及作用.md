---
title: Linux man 后数字涵义及作用
date: 2017-11-06 21:18:49
categories: Linux
tags: [man]
---
平时我们开发中碰到陌生的命令或者函数，通常会找man来帮助下，通过man后面加的数字可以看到关于命令或者函数更深层次的东西。
或者通过比较命令和函数加深了解你需要的知识。
如下是man后面的数字代表的含义。
Section 1
user commands (introduction)
Section 2
system calls (introduction)
Section 3
library functions (introduction)
Section 4
special files (introduction)
Section 5
file formats (introduction)
Section 6
games (introduction)
Section 7
conventions and miscellany (introduction)
Section 8
administration and privileged commands (introduction)
Section L
math library functions
Section N
tcl functions

中文含义如下：
             1 用户命令， 可由任何人启动的。
2 系统调用， 即由内核提供的函数。

3 例程， 即库函数。

4 设备， 即/dev目录下的特殊文件。

5 文件格式描述， 例如/etc/passwd。

6 游戏， 不用解释啦！

7 杂项， 例如宏命令包、惯例等。

8 系统管理员工具， 只能由root启动。

9 其他（Linux特定的）， 用来存放内核例行程序的文档。

n 新文档， 可能要移到更适合的领域。

o 老文档， 可能会在一段期限内保留。

l 本地文档， 与本特定系统有关的。
