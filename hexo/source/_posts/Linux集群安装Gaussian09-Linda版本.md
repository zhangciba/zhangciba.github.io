---
title: Linux集群安装Gaussian09 Linda版本
date: 2017-06-03 12:56:09
categories: 集群管理
tags: [集群管理,高性能计算,Gaussian,Gaussian09]
---
### 解压文件
执行如下命令，解压gaussian09软件包到/public/software目录下面，得到目录/public/software/g09
```
tar -zxvf g09_linux.tgz  /public/software/
```
具体解压路径可以根据自己的实际情况进行更换，例如也可以安装到/usr/local/目录下面，若是希望进行并行化运行，那么最好安装到分布式文件系统目录中。


<!--more-->
### 修改环境变量文件  
#### 设置环境变量
如果是root用户，安装后希望所有用户都可以使用该软件，则在/etc/profile.d/目录中添加g09.sh文件，并输入如下内容：
如果是普通用户，则在用户的主目录/home/username/.bashrc文件的结尾处如下内容：
```
#Gaussian 09
export PATH=/public/software/g09:$PATH
export g09root=/public/software/g09
export GAUSS_EXEDIR=$g09root
export GAUSS_SCRDIR=$g09root/scratch
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$g09root/
source $g09root/bsd/g09.profile
```
其中/public/software/g09是gaussian软件的解压目录，将所有的/public/software/g09替换为自己的安装目录即可。

#### 设置csh文件
如果是root用户，安装后希望所有用户都可以使用该软件，则在/etc/profile.d/目录中添加g09.csh文件，并输入如下内容：
如果是普通用户，则在用户的主目录/home/username/.cshrc文件的结尾处如下内容：

```
#For gaussian 09
setenv g09root /public/software
setenv GAUSS_SCRDIR /public/software/g09/scratch
setenv GAUSS_LFLAGS '-vv -nodelist "node181 node182 node188 node189"'
source /public/software/g09/bsd/g09.login
```
将node181 - node189修改为需要并行运行Gaussian09的机器的主机名

### 修改文件夹权限
将gaussian09的安装目录的权限修改为710
```
chmod -R 710 /public/software/g09
```

并将该文件夹的属主和用户组修改为需要执行gaussian09的用户组
```
chown -R users /public/software/g09
#将users修改为自己的用户名
chgrp -R users /public/software/g09
#将users修改为自己的用户组
#可以通过执行id命令，输出结果中的groups信息得到
```

进入gaussian09的安装目录，然后创建文件夹scratch，并赋予权限
```
cd /public/software/g09
mkdir scratch
chmod -R 777 scratch
```

进入gaussian09安装目录，然后修改bsd文件夹的权限
```
chmod -R 755 /public/software/g09/bsd
```

进入gaussian09安装目录，然后修改tests文件夹的权限
```
chmod -R 777 /public/software/g09/tests
```

### 试运行
```
g09 < /public/software/g09/tests/com/test0001.com
```

输出结果如下：
若看到如下输出，则就是正确安装了。
```
Normal termination of Gaussian 09 at Sat Jun  3 16:36:49 2017.
```


```
[zhangchi@node307 g09]$ g09  < ./tests/com/test0006.com
 Entering Gaussian System, Link 0=g09
 Initial command:
 /public/software/g09/l1.exe "/public/software/g09/scratch/Gau-8557.inp" -scrdir="/public/software/g09/scratch/"
 Entering Link 1 = /public/software/g09/l1.exe PID=      8558.

 Copyright (c) 1988,1990,1992,1993,1995,1998,2003,2009,2013,
            Gaussian, Inc.  All Rights Reserved.

 This is part of the Gaussian(R) 09 program.  It is based on
 the Gaussian(R) 03 system (copyright 2003, Gaussian, Inc.),
 the Gaussian(R) 98 system (copyright 1998, Gaussian, Inc.),
 the Gaussian(R) 94 system (copyright 1995, Gaussian, Inc.),
 the Gaussian 92(TM) system (copyright 1992, Gaussian, Inc.),
 the Gaussian 90(TM) system (copyright 1990, Gaussian, Inc.),
 the Gaussian 88(TM) system (copyright 1988, Gaussian, Inc.),
 the Gaussian 86(TM) system (copyright 1986, Carnegie Mellon
 University), and the Gaussian 82(TM) system (copyright 1983,
 Carnegie Mellon University). Gaussian is a federally registered
 trademark of Gaussian, Inc.

 This software contains proprietary and confidential information,
 including trade secrets, belonging to Gaussian, Inc.

 This software is provided under written license and may be
 used, copied, transmitted, or stored only in accord with that
 written license.

 The following legend is applicable only to US Government
 contracts under FAR:

                    RESTRICTED RIGHTS LEGEND

 Use, reproduction and disclosure by the US Government is
 subject to restrictions as set forth in subparagraphs (a)
 and (c) of the Commercial Computer Software - Restricted
 Rights clause in FAR 52.227-19.

 Gaussian, Inc.
 340 Quinnipiac St., Bldg. 40, Wallingford CT 06492


 ---------------------------------------------------------------
 Warning -- This program may not be used in any manner that
 competes with the business of Gaussian, Inc. or will provide
 assistance to any competitor of Gaussian, Inc.  The licensee
 of this program is prohibited from giving any competitor of
 Gaussian, Inc. access to this program.  By using this program,
 the user acknowledges that Gaussian, Inc. is engaged in the
 business of creating and licensing software in the field of
 computational chemistry and represents and warrants to the
 licensee that it is not a competitor of Gaussian, Inc. and that
 it will not use this program in any manner prohibited above.
 ---------------------------------------------------------------


 Cite this work as:
 Gaussian 09, Revision D.01,
 M. J. Frisch, G. W. Trucks, H. B. Schlegel, G. E. Scuseria,
 M. A. Robb, J. R. Cheeseman, G. Scalmani, V. Barone, B. Mennucci,
 G. A. Petersson, H. Nakatsuji, M. Caricato, X. Li, H. P. Hratchian,
 A. F. Izmaylov, J. Bloino, G. Zheng, J. L. Sonnenberg, M. Hada,
 M. Ehara, K. Toyota, R. Fukuda, J. Hasegawa, M. Ishida, T. Nakajima,
 Y. Honda, O. Kitao, H. Nakai, T. Vreven, J. A. Montgomery, Jr.,
 J. E. Peralta, F. Ogliaro, M. Bearpark, J. J. Heyd, E. Brothers,
 K. N. Kudin, V. N. Staroverov, T. Keith, R. Kobayashi, J. Normand,
 K. Raghavachari, A. Rendell, J. C. Burant, S. S. Iyengar, J. Tomasi,
 M. Cossi, N. Rega, J. M. Millam, M. Klene, J. E. Knox, J. B. Cross,
 V. Bakken, C. Adamo, J. Jaramillo, R. Gomperts, R. E. Stratmann,
 O. Yazyev, A. J. Austin, R. Cammi, C. Pomelli, J. W. Ochterski,
 R. L. Martin, K. Morokuma, V. G. Zakrzewski, G. A. Voth,
 P. Salvador, J. J. Dannenberg, S. Dapprich, A. D. Daniels,
 O. Farkas, J. B. Foresman, J. V. Ortiz, J. Cioslowski,
 and D. J. Fox, Gaussian, Inc., Wallingford CT, 2013.

 ******************************************
 Gaussian 09:  ES64L-G09RevD.01 24-Apr-2013
                 3-Jun-2017
 ******************************************
中间省略了很多信息		    
中间省略了很多信息		    
中间省略了很多信息		    
中间省略了很多信息		    
中间省略了很多信息		    
中间省略了很多信息		    
****
 Centers:       2      3
 S 3 1.20
     Exponent=  1.3007734000D+01 Coefficients=  3.3494604340D-02
     Exponent=  1.9620794200D+00 Coefficients=  2.3472695350D-01
     Exponent=  4.4452895300D-01 Coefficients=  8.1375732600D-01
 S 1 1.15
     Exponent=  1.2194915600D-01 Coefficients=  1.0000000000D+00
 ****
 .003e-09\Dipole=0.686999,0.,0.4857817\Quadrupole=0.2774616,-1.1043451,
 0.8268835,0.,-0.777,0.\PG=C02V [C2(O1),SGV(H2)]\\@


 THE FIRST AND LAST THING REQUIRED OF GENIUS IS THE LOVE OF TRUTH.

                          -- GOETHE
 Job cpu time:       0 days  0 hours  0 minutes  1.5 seconds.
 File lengths (MBytes):  RWF=      5 Int=      1 D2E=      0 Chk=      1 Scr=      1
 Normal termination of Gaussian 09 at Sat Jun  3 16:36:49 2017.
```

#### 配置Linda
将/public/software/g09/linda8.2/common/lib/tsnet.config中的rsh改为ssh
```
Tsnet.Node.lindarsharg: rsh
```
修改为
```
Tsnet.Node.lindarsharg: ssh
```

参考文献
[Gaussian官方文档](http://www.surfchem.fudan.edu.cn/teacher/lizh/Usefull_Files/g09/g_tech/g_ur/m_cfenv.htm)
[Gaussian官方文档](http://www.surfchem.fudan.edu.cn/teacher/lizh/Usefull_Files/g09/g_tech/g_ur/m_linda.htm)
