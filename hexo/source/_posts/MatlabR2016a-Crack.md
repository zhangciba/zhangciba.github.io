---
title: MatlabR2016a Crack
date: 2017-11-10 09:30:35
categories: 集群管理
tags: [Matlab,Cluster]
---

#### 准备工作
1. 首先要下载好Matlab R2016a及其破解文件;
2. 将下载好的Matlab R2016a及其破解文件解压;

#### 安装过程
1. 建立一个文件夹，直接挂载.iso文件。
sudo mkdir /mnt
用来挂载.iso文件，就跟windows里的虚拟光驱一样;

2. cd 到iso所在的路径，也就是你解压后的那个文件夹，存放iso的;

3. sudo mount -o loop R2016a_glnxa64.iso /mnt 把iso挂载到刚建的虚拟光驱中;

4. 安装
$cd /mnt/ 
sudo ./install
5. 接下来会打开matlab的安装界面(与Windows下一样)，然后输入密钥。然后一直下一步，直到安装完成，大约耗时1个小时。

#### Crack过程
1. 创建licenses文件夹
进入MatlabR2016a的安装文件夹
```
cd /usr/local/MATLAB/R2016a/bin
```

新建“licenses”文件并赋予其权限
```
sudo mkdir licenses
sudo chmod 777 licenses
```
<!--more-->
2. 覆盖文件
首先把/usr/local/MATLAB/R2016a/bin/glnxa64下面原来的两个文件libmwservices.so和libcufft.so.7.5.18复制到其他文件夹。
其次，将linux破解文件夹MATLAB R2016a_glnxa64_crack目录下的
Matlab_R2016a_glnxa64.lic
libmwservices.so
libcufft.so.7.5.18

分别拷贝到上述新建的”licenses”和”./bin/glnxa64”下面 
```
sudo cp Matlab_R2016a_glnxa64.lic /usr/local/MATLAB/R2016a/bin/licenses 
sudo cp libmwservices.so /usr/local/MATLAB/R2016a/bin/glnxa64 
sudo cp libcufft.so.7.5.18 /usr/local/MATLAB/R2016a/bin/glnxa64
```

3. 到此为止，在Ubuntu 16.04下安装MATLAB R2016a就完成了，我们只要从命令行进入matlab R2016a的”bin”目录，输入
```
cd /usr/local/MATLAB/R2016a/bin
./matlab
```
打开时，会弹出激活窗口，选择不使用Internet进行激活，进入一个License选择界面，选择/usr/local/MATLAB/R2016a/bin/licenses的Matlab_R2016a_glnxa64.lic后， 即可正常使用MatlabR2016a了。
