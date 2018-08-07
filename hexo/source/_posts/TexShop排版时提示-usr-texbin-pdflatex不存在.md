---
title: TexShop排版时提示/usr/texbin/pdflatex不存在
date: 2016-04-27 19:31:54
categories: LaTex
tags:
   - Tex
   - TexShop
---

LaTeX是论文写作的利器，可以极大方便我们的排版工作，因为只用几行代码就可以做出非常好的排版效果。但是学习起来成本确实很高。这不，今天我刚开始使用的时候就遇到了问题。
在MacOSX系统下面，我们使用LaTeX的软件有很多，但是我推荐初学者使用TeXLive或者是基于TeXLive进行的封装MacTex，我更加推荐的是MacTeX，因为其封装了一些LaTeXiT、BibDesk、TeXShop、Tex Live Utility，省去了很多配置方面的麻烦。
<!-- more -->

但是安装之后，我们打开TeXShop进行写作之后，点击排版，会提示一个错误，错误如下所示。
```
/usr/texbin/pdflatex 不存在。TeXLive 可能未安装
```
提示这个错误的原因就是/usr/texbin/中找不到padlatex这个文件，其实/usr/texbin这个文件夹就不存在。

出现这个问题的原因是由于OSX系统升级到CaptionEI之后，用户没有权限在目录/usr下面创建文件或者文件夹。但是MacTeX在进行安装的时候创建了这个软连接/Library/texbin指向了/usr/local/texlive/2015/x86_64-darwin/下面，由于在TeXShop中的Preference设置中的Engine的路径设置没有修改过来，所以就导致了在进行排版的时候找不到padlatex或者是xelatex等可执行文件了。

解决办法就是修改TeXShop->Preference(偏好)->Engine(引擎)中的路径设置中的/usr/texbin/修改为/Library/TeX/texbin就可以了。
其他的TeX编辑软件如果出现这样的情况，也可以采用相同的设置方法，将tex的路径修改为/Library/TeX/texbin就可以了。

参考官方文档[ UpdatingForEIcaptian][1]
[1]: http://7xsnoh.com2.z0.glb.clouddn.com/UpdatingForElCapitan.pdf

