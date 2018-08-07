---
title: Linux终端格式化输出JSON
date: 2016-04-11 18:47:21
tags: JSON格式化输出
categories: Linux
---

&#160;&#160;我们在使用curl命令访问某一个页面时，往往会返回一个JSON字符串，默认情况下，Linux、Unix、MacOSX等系统下面的终端是不会格式化输出的，我们在查看结果的时候会比较吃力。有什么办法可以让JSON可以格式化输出呢。以CentOS7为例，CentOS7默认会安装Python2,我们在使用curl命令时，在它后面加入一下命令即可.
```
curl http://www.xxx.com|python -m json.tool
```

&#160;&#160;后来在网上又看到了另外一个比较好用的工具，jq，<https://stedolan.github.io/jq/>，具体的使用方法可以参考我给的链接啦。
<!-- more -->

