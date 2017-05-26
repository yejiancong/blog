# php-fpm进程设置多少合适，设成动态还是静态？

## /usr/local/php/etc/php-fpm.conf重要优化参数详解：

```pm = dynamic```

pm参数指定了进程管理方式，有两种可供选择：static或dynamic，从字面意思不难理解，为静态或动态方式。如果是静态方式，那么在php-fpm启动的时候就创建了指定数目的进程，在运行过程中不会再有变化(并不是真的就永远不变)；而动态的则在运行过程中动态调整，当然并不是无限制的创建新进程，受pm.max_spare_servers参数影响；动态适合小内存机器，灵活分配进程，省内存。静态适用于大内存机器，动态创建回收进程对服务器资源也是一种消耗

```pm.max_children = 24```

static模式下创建的子进程数或dynamic模式下同一时刻允许最大的php-fpm子进程数量

```pm.start_servers = 16```

动态方式下的起始php-fpm进程数量

```pm.min_spare_servers = 12```

动态方式下服务器空闲时最小php-fpm进程数量

```pm.max_spare_servers = 24```

## 动态方式下服务器空闲时最大php-fpm进程数量

一般php-fpm进程占用20~30m左右的内存就按30m算。如果单独跑php-fpm，动态方式起始值可设置物理内存Mem/30M，由于大家一般Nginx、MySQL都在一台机器上，于是预留一半给它们，即php-fpm进程数为$Mem/2/30。
LNMP在一台机器上参数（仅供参考，建议压力测试得出）：

```
Mem=`free -m | awk '/Mem:/{print $2}'` #我的机器内存是987M
sed -i "s@^pm.max_children.*@pm.max_children = $(($Mem/2/20))@" $php_install_dir/etc/php-fpm.conf
sed -i "s@^pm.start_servers.*@pm.start_servers = $(($Mem/2/30))@" $php_install_dir/etc/php-fpm.conf
sed -i "s@^pm.min_spare_servers.*@pm.min_spare_servers = $(($Mem/2/40))@" $php_install_dir/etc/php-fpm.conf
sed -i "s@^pm.max_spare_servers.*@pm.max_spare_servers = $(($Mem/2/20))@" $php_install_dir/etc/php-fpm.conf
```

987M内存：
```
pm = dynamic
pm.max_children = 24
pm.start_servers = 16
pm.min_spare_servers = 12
```

自定义输出
```
i=1 #倍数
Mem=`free -m | awk '/Mem:/{print $2}'` #我的机器内存
echo $Mem
echo pm.max_children = $(($Mem/$i/20))
echo pm.start_servers = $(($Mem/$i/30))
echo pm.min_spare_servers = $(($Mem/$i/40))
echo pm.max_spare_servers = $(($Mem/$i/20))
```

## 用strace 调试 php-fpm进程

查看php-fpm进程
```
ps -ef | grep php-fpm
```

调试进程输出日志到文件
```
sudo strace -f -p 2105 -e trace=file -o /temp/trace.log
```

查看日志文件
```
tail -f /temp/trace.log
```
