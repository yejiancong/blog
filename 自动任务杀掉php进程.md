
#检查php进程的内存占用，杀掉内存使用超额的进程

一般情况下，如果php-cgi进程占用超过10%的内存，就得考虑一下是否要杀掉它了。因为普通情况下，php-cgi进程一般占用0.2%或以下。

这里提供一个脚本供各位使用，就是放在cron任务里，每分钟执行一次。

```shell
cd /root/
vi kill_php_cgi.sh
chown +x ./kill_php_cgi.sh
```


```
#!/bin/sh
# This script is used to kill php-cgi process that takes large memory size
# If a php-cgi process uses 1% or more memory, then it will be killed.

PIDS=`ps aux|grep php-fpm|grep -v grep|awk '{if($4>=10)print $2}'`

for PID in $PIDS
do
#echo `date +%F....%T` >> /data/local/php/logs/phpkill.log
#echo $PID >> /usr/local/php/logs/phpkill.log
kill -9  $PID
done
```

使用crontab -e 命令，然后添加如下调度任务

```
* * * * * /bin/bash /root/kill_php_cgi.sh
```

##Crontab用法

```
[root@bash ~]# crontab --help   #<==注：crontab –l –e都是直接操作/var/spool/cron/下当前用户名的文件
usage: crontab [-u user] file    #<==指定某用户如crontab –u yejiancong –e，编辑yejiancong家目录下的crontab
    crontab [-u user] [ -e | -l | -r ]
        (default operation is replace,per 1003.2)
    -e   (edit user's crontab)  #<==编辑当前用户的定时任务
    -l   (list user's crontab)  #<==查看当前用户的定时任务
    -r   (delete user's crontab)  #<==删除定时任务
    -i   (prompt before deletinguser's crontab) #<==删除crontab文件内容，删前会有提示
    -s   (selinux context)
```
cron执行的每一项工作都会被纪录到/var/log/cron这个日志文件中，可以从这个文件查看命令执行的状态。
