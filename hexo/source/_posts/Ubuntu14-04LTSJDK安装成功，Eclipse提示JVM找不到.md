---
title: Ubuntu14.04LTSJDK安装成功，Eclipse提示JVM找不到
date: 2016-04-03 09:30:26
tags: JVM
categories: Linux
---

### 问题描述
Ubuntu14.04已经安装JDK成功，但是启动Eclipse的时候提示如下错误

报 A Java Runtime Environment (JRE) or Java Development Kit (JDK)
must be available in order to run Eclipse. No Java virtual machine
was found after searching the following locations:
/opt/eclipse/jre/bin/java
java in your current PATH这错误
但我在终端运行java -version
java version "1.6.0_26"
Java(TM) SE Runtime Environment (build 1.6.0_26-b03)
Java HotSpot(TM) Server VM (build 20.1-b02, mixed mode)
### 可能原因
1.JDK版本和Eclipse版本不合适，例如JDK是32位，Eclipse是64位，或者JDK是64位，Eclipse是32位
2.JDK环境变量没有进行配置。
### 解决办法

Eclipse有个自动查找本地JVM环境，这个错误就是说eclipse没能找到必须得jvm运行环境，这个时候，我们可以手动为其指定jvm的路径，在eclipse安装文件夹下，有一个eclipse.ini的初始化配置文件，用文本编辑工具打开，追加“-vm”参数：
-vm $YOUR_JAVA_HOME/bin/javaw
这里的$YOUR_JAVA_HOME 是你的java安装路径。必须保证javaw程序在这个环境下，这样，eclipse启动时会通过指定的jvm启动IDE（有的机器上安装多个版本的java，也可以这样来配置）。


