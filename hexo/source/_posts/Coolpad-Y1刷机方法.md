---
title: Coolpad Y1刷机方法
date: 2016-06-01 16:05:49
categories: 手机刷机
tags: Coolpad Y1
---

说明，本方法可以刷不能正常进入系统的手机，就是俗称的砖头机，也可以刷正常的手机，你值得拥有。

刷机步骤如下：

#### 准备工作

##### 安装酷派手机驱动
  [点我下载酷派驱动](http://pan.baidu.com/s/1bpDObkb "酷派手机驱动下载")，下载之后，安装一下。

##### 下载酷派Y1手机对应的刷机包
  这篇文章介绍了酷派目前有的刷机包，可以前去下载[点我查看刷机包帖子](http://tieba.baidu.com/p/4136625814 "刷机包链接")，请下载官方的刷机包，这样才可以用Coolpad Download Assistant来进行刷机。下载完成之后，解压刷机包，可以看到一个后缀名是.cpb的文件，这个就是刷机包了。
<!-- more -->

##### 下载酷派手机刷机工具
  刷机工具名字叫做Coolpad Download Assistant（酷派下载助手），下载链接地址是[点我下载Coolpad Download Assistant](http://pan.baidu.com/s/1o84EiwU "Coolpad Download Assistant" )
  下载之后就可以安装了，过程就是一路下一步，安装之后，桌面会出现一个蓝色的上传标识的图标，就是软件的快捷方式。

#### 开始刷机
  当驱动安装好了，CPB刷机包下载之后，再就是电量超过50%之后，就可以使用Coolpad Download Assistant刷机了。
  
  *请一定要注意，电量一定要超过50%，否则刷机不成功可能导致芯片损坏。*
  *请一定要下载官方的线刷包，否则如果不包含.cpb文件则不能使用这种方法刷机。*

##### 手机连接电脑
  酷派Y1手机的进入Fastboot的方式是这样的。手机在完全关机的情况下，可以先把电池卸载下来，然后装上去，请注意不要插上数据线。然后按着音量增大的键，一直按着，然后用数据线将手机连上电脑，出现下面的界面之后再松开。
  ![fastboot界面](http://cdn.zhangchi.xyz/coolpad_fastboot1.jpg)
  
  出现这个界面之后，再按一下音量加，进入如下界面表示可以进行刷机了。
  ![fastboot界面](http://cdn.zhangchi.xyz/coolpad_fastboot2.jpg)
  *请一定要保证手机电量充足，另外保证手机连接稳定，以免刷机过程中出现连接中断导致刷机失败的情况。*
  为了验证驱动是否安装成功，在Windows系统下面的设备管理器里面应该可以看到下面图片中的设备。打开设别管理器的方法是在计算机图标上面点击右键，出现设备管理器就可以进入了。
  ![设备管理器中的酷派手机](http://cdn.zhangchi.xyz/device_success.png)
  如果没有出现，点击计算机名，右键，扫描硬件改动即可。如果还是不行的话，可以重新安装驱动，然后重新将手机以fastboot方式连接电脑。（再次重复，将手机完全关机，按住音量加号键，插上数据线连接电脑，图片如前面所示）  

##### 解压CPB线刷包
  一般刷机包都是以压缩包的方式存在的，下载完成之后，解压，得到一个以.cpb为后缀的文件。
  如果没有.cpb文件，则表示不是下载的官方线刷包，请注意，一定要下载官方线刷包。
 
##### 使用刷机工具进行刷机
  首先打开Coolpad Download Assistant软件，界面如下：
  ![软件界面](http://cdn.zhangchi.xyz/cds_step1.png)
  *确保手机已经连接上了*，连接方式参考之前的步骤

  其次选择刷机包文件，如图所示：
  ![选择刷机包文件](http://cdn.zhangchi.xyz/cds_step3.png)
	
  然后开始刷机包验证，如图所示：
  ![压缩包验证](http://cdn.zhangchi.xyz/cds_step4.png)
  ![压缩包验证](http://cdn.zhangchi.xyz/cds_step5.png)
 
  校验完成之后就开始刷机了，如图所示：
  ![刷机过程](http://cdn.zhangchi.xyz/cds_step6.png)

  等待10分钟左右，刷机完成，如图所示：
  ![刷机完成](http://cdn.zhangchi.xyz/cds_step7.png)

#### 刷机之后
  启动手机之后会卡在中国电信的欢迎界面，这个时候，可以拔掉电池，然后重新启动，就可以进入系统了。
  OK，刷机成功了，尽量享受吧，开机之后，可以使用其他工具刷机了。
