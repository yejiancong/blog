
#检查php进程的内存占用，杀掉内存使用超额的进程

一般情况下，如果php-cgi进程占用超过1%的内存，就得考虑一下是否要杀掉它了。因为普通情况下，php-cgi进程一般占用0.2%或以下。

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
