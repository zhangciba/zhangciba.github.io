---
title: 'java.lang.NoClassDefFoundError: Could not initialize class '
date: 2016-10-17 23:35:49
categories: Java
tags: [Java,NoClassDefFoundError]
---

今天在部署一个其他人开发的项目时，遇到了一个问题，就是有一个页面的查询功能不能正常使用，由于该页面采用的是Ext.js框架，服务器端返回的数据是JSON格式，由于报错了，导致无任何JSON数据返回，页面估计也没有做任何超时判断，因此也没有提示错误信息。
打开该页面对应的js文件，找到了对应按钮的函数，并确定了对应的后台方法的URL，在浏览器中进行输入URL发送请求，发现还是报错了。 
报错的root cause是java.lang.NoClassDefFoundError: Could not initialize class,后面的Class是一个包含了很多静态方法的工具类，该工具类的功能是根据IP地址返回具体的城市信息。
<!--more-->

由于报错提示是java.lang.NoClassDefFoundError: Could not initialize class,所以第一就是根据这个错误提示去搜索解决办法。不过搜索解决办法之前，我还是去Tomcat容器里面找了一下，发现WEB-INF里面确实有这个类。所以不知道是什么原因导致的。

按照网上很多人说的，这种报错一般会有如下几种原因：
1.这个类确实不存在，可能有如下几种原因：
 a.编译出错，编译的编码或者版本与Tomcat容器使用的JRE版本相差太大。
 b.类文件压根没有编译。

2.同时存在两个名字一模一样的.class文件，这种情况一般是如下几种原因导致：
 a.导入其他框架或者外置的不同的.jar文件中，存在两个相同名字的类文件，而且路径一直。这种情况一般称为包冲突。
   例如，Hibernate框架和Spring框架都会使用到asm.jar文件，如果同时导入这两个框架的.jar文件，会导致asm相关的类找不到，报文章题目    中的错误。
 b.Tomcat容器的lib目录下面存在和Web项目的lib目录下面同样的类文件。

3.类文件存在，但是没有正常的加入到项目的Build Path中，有时候项目在不同版本的IDE之间导入导出，导致一些配置信息不能正常生效，所以即使加入了正确的.jar文件，也找不到里面的类文件。

这些方法中都是在描述.class文件是否存在以及存在多个导致冲突的问题。 

后来直接跑到服务器端的CONSOLE中看到了该URL请求时的报错，首先说明一个这个URL中对应的功能是记录用户的登录信息，根据登录IP获取登录地址，并记录到数据库的日志表中。因此需要将IP地址和本地IP文件库中进行比对，从而判断IP所在省市，最后发现是文件找不到导致的错误。后来调整了文件的路径，该问题就得到了解决。

是不是觉得很奇怪，一个静态类中的一个方法因为一个文件找不到却报java.lang.NoClassDefFoundError错误。
