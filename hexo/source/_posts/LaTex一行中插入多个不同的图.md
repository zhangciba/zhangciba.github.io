---
title: LaTex一行中插入多个不同的图
date: 2016-06-14 17:54:44
categories: LaTex
tags: LaTeX图片
---

```
\begin{figure}[htbp]
\begin{minipage}[t]{0.3\linewidth}
\centering
\includegraphics[width=2.2in,height=2.0in]{1.eps}
\caption{fig1}
\label{fig:side:a}
\end{minipage}%
\begin{minipage}[t]{0.3\linewidth}
\centering
\includegraphics[width=2.2in,height=2.0in]{2.eps}
\caption{fig2}
\begin{minipage}[t]{0.3\linewidth}
\centering
\includegraphics[width=2.2in,height=2.0in]{3.eps}
\caption{fig2}
\label{}
\end{minipage}
\end{figure} 
在\caption{}中填写图形的注释
```

