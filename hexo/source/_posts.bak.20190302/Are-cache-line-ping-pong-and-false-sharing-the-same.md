---
title: Are cache-line-ping-pong and false sharing the same?
date: 2018-02-25 09:14:46
categories: Linux
tags: [cache ping-pong, false sharing]
keywords: ping-pong
description: Are cache-line-ping-pong and false sharing the same
---

Summary:

False sharing and cache-line ping-ponging are related but not the same thing. False sharing can cause cache-line ping-ponging, but it is not the only possible cause since cache-line ping-ponging can also be caused by true sharing.

Details:

False sharing

False sharing occurs when different threads have data that is not shared in the program, but this data gets mapped to a cache line that is shared. For example imagine a program that had an array of integers where one thread performed reads and writes to all of the array entries with an even index, and the other thread performed reads and writes to entries with an odd index. In this case the threads would not actually be sharing data, but they would share cache lines since each cache line would contain both odd and even indexed values (assuming the cache line was bigger than an integer, which is typically true).

Cache-line ping-ponging

Cache line ping-ponging is the effect where a cache line is transferred between multiple CPUs (or cores) in rapid succession. This can be cause by either false or true sharing. Essentially if multiple CPUs are trying to read and write data in the same cache line then that cache line might have to be transferred between the two threads in rapid succession, and this can cause a significant performance degradation (possibly even worse performance than if a single thread were executing). False sharing can make this problem particularly difficult to detect, because a programmer might have tried to write an application so that the threads weren't sharing data, without realizing that the data was mapped to the same cache line. But false sharing is not the only possible cause of cache-line ping-ponging. This could also be caused by true sharing where multiple threads are trying to read and write the same data.


From [StackOverFlow](https://stackoverflow.com/questions/30684974/are-cache-line-ping-pong-and-false-sharing-the-same)

