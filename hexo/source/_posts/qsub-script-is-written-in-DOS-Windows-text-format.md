---
title: 'qsub:script is written in DOS/Windows text format'
date: 2017-11-15 16:27:06
categories: Linux
tags: [HPC,Torque,Linux]
---

dos格式文件传输到unix系统时，会在每行的结尾多一个^M（/r），当然也有可能看不到。但是在vim的时候，会在下面显示此文件的格式，比如 "dos.txt" [dos] 120L, 2532C 字样,表示是一个[dos]格式文件，如果是MAC系统的，会显示[MAC]。因为文件格式的原因有时会导致我们的unix程序，或者shell程序出现错误，那么需要把这些dos文件格式转换成unix格式，方法是
``` 
vim dos.txt
:set fileformat=unix
:w
```

