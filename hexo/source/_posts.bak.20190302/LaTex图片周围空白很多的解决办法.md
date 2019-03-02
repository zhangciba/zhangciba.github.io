---
title: LaTex图片周围空白很多
date: 2016-06-15 09:01:39
categories: LaTex
tags: LaTeX图片
---

插入至LaTeX中的图片的周围的空白占用的空间很多，是什么愿意导致的呢。可能有一下几种原因：
* 图片本身空白空间很大。
* 当有图表时，如果图表占用的空间超过70%，则LaTeX不会再页面中添加文本

<!--more-->
对于第一种情况，先把图片进行裁剪之后，然后再插入，如果还是占用很大空白空间，则按第二种情况进行处理。

对于第二种情况，首先尝试使用下面的办法来解决，代码如下:
```
\begin{figure}[H]
\centering
\includegraphics[width=4.in]{1.eps}
\caption{XXXXXXX}\label{1}
\end
```
就是在begin{figure}后面加'[H]'一般可以解决。

如果还是不行，可以在导言区加入以下代码进行解决，课参考[博客园](http://www.cnblogs.com/yishuiliunian/archive/2011/03/23/1992073.html)
```
\renewcommand\floatpagefraction{.9}
\renewcommand\topfraction{.9}
\renewcommand\bottomfraction{.9}
\renewcommand\textfraction{.1}
\setcounter{totalnumber}{50}
\setcounter{topnumber}{50}
\setcounter{bottomnumber}{50}
```

