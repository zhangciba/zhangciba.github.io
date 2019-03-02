---
title: 让MyEclipse10支持视网膜屏幕
date: 2016-05-05 19:01:46
categories: Mac OSX
tags: myeclipse10支持视网膜屏幕
toc: true
---

# 摘要
自从苹果除了视网膜屏之后，视觉效果提升了一个档次。但是很多软件在开发时就没有支持视网膜屏幕，而另外的一些软件在开发时做了对支持视网膜屏幕，但是默认设置是不开启高分辨率的，需要进行修改才可以支持视网膜屏幕。
<!-- more -->
其实视网膜屏幕就是一块分辨率比我们现在使用的屏幕更高的屏幕罢了，它的像素点个数是普通屏幕的四倍以上，所以显示更加清晰。
所以当大家使用其他的软件时，遇到模糊的问题也可以按照我的思路来进行参考。

# 具体解决办法如下：

## 首先打开Finder，进入到应用程序的目录。
如图1所示
![打开应用程序目录](http://cdn.zhangchi.xyz/myeclipse01.png)

## 编辑Info.plist文件
然后进入MyEclipse的目录下面，可以看到MyEclipse10的图标，双手指点按，出来菜单，选择显示包内容，我们可以看到Info.plist文件，用XCode打开，点击‘+’号，添加一个键NSHighResolution Capable，并设置值为YES，保存，然后退出XCode。
如果使用其他编辑软件打开的，那么在'</dict></plist>'之前，加入如下内容（不包含单引号）
```
<key>NSHighResolutionCapable</key>
	<true/>
```
## 进入Profile文件夹编辑另外一个Info.plist
这个时候并没有结束，网上很多都只介绍到这里，导致很多人都没有成功。接下来，打开Profile文件夹，可以看到，还有一个MyEclipse，按照之前的方法，也是讲Info.plist文件中加入NSHighResolutionCapable这个键，并设置值为YES即可。

## 拷贝一份程序，删除原来的程序
最后一步，就是把MyEclipse.app这个文件复制一份，得到MyEclipse副本.app，然后把MyEclipse.app删除，再把MyEclipse副本重命名为MyEclipse即可，重新打开之后，就可以看到MyEclipse10界面变得很清晰了。

## 过程截图

![显示包内容](http://cdn.zhangchi.xyz/myeclipse02.png)
![显示包内容](http://cdn.zhangchi.xyz/myeclipse03.png)
![打开Profile文件夹](http://cdn.zhangchi.xyz/myeclipse04.png)

![再次显示包内容](http://cdn.zhangchi.xyz/myeclipse05.png)
![编辑Info.plist文件](http://cdn.zhangchi.xyz/myeclipse06.png)
![编辑Info.plist文件](http://cdn.zhangchi.xyz/myeclipse07.png)

