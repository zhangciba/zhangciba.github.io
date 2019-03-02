---
title: Linux Shell读取配置文件2
date: 2016-06-08 23:58:08
categories: Shell
tags: 读取配置文件
---

还有另外一种形式的配置文件，可能没有分段，稍微简单一点，因此，也可以用简单一点的方法进行读取配置文件的信息。

例如config.ini的内容如下：
```
ip=8.8.8.8
username=Jim
password=123456
dir=/root/
```
<!--more-->
这种形式的配置文件的读取方法如read.sh脚本所示：
```
#!/bin/sh

#DIR变量为当前shell脚本的目录
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
#使用source命令导入平级目录下的配置文件,引入以后就可以直接用配置中的变量名来获取文件中的值了
source $DIR"/"config.ini
echo "ip=${ip}";
echo "usernmae=${username}";
echo "password=${password}";
```

需要说明的是，配置文件和脚本放在同一个目录下，所以需要获取到当前目录的路径。然后在读取配置文件。这种方式读取配置文件要比使用正则表达是稍微简单一些，但是复杂一点的程序的配置文件一般没有这么简单，所以还是学习正则表达式之后来灵活处理这种配置文件更加实用一些。

下面附录一段通过这种简单配置文件来实现备份mysql数据库的shell脚本。如下所示：
```
#!/bin/bash
# Name:bak_ftp_daily.sh
# 自动备份数据库，并删除旧的备份文件
# DIR变量为当前shell脚本的目录
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
# 使用source命令导入平级目录下的配置文件,引入以后就可以直接用配置中的变量名来获取文件中的值了
source $DIR"/"db_config

# 生成时间戳
time=_` date +%Y_%m_%d_%H_%M_%S `_
echo "------bakup---<<<--`date +%Y-%m-%d-%H-%M-%S`----begin--->>>---";

# 生成备份文件名
echo $day_backupdir/$db_name$time$day_backup_fix.sql.gz"---is --begin---";

# 执行备份命令并压缩文件
mysqldump -u $db_user -p $db_pass $db_name | gzip > $day_backupdir/$db_name$time$day_backup_fix.sql.gz

echo $day_backupdir/$db_name$time$day_backup_fix.sql.gz"---is --finsh---";

#查找早些的配置文件，并删除
find $day_backupdir -name $db_name"*.sql.gz" -type f -mmin +$day_interval -exec rm -rf {} \; > /dev/null 2>&1
echo "------bakup----<<<---`date +%Y-%m-%d-%H-%M-%S`----finsh--->>>---";
echo "";
```

将这个脚本添加到Linux Crontab中，配置好执行频率，即可实现自动备份了。
