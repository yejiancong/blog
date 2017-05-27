# 安装memcached服务端

```
cd $ROOTDIR
tar xvf memcached-1.4.20.tar.gz
cd memcached-1.4.20
./configure --prefix=/usr/local/memcached
make && make install
killall memcached
/usr/local/memcached/bin/memcached -d -m 10 -u www -l 127.0.0.1 -p 11211 -c 256 -P /tmp/memcached.pid
#-d选项是启动一个守护进程，
#-m是分配给Memcache使用的内存数量，单位是MB，我这里是10MB，
#-u是运行Memcache的用户，我这里是www，
#-l是监听的服务器IP地址，如果有多个地址的话，我这里指定了服务器的IP地址127.0.0.1，
#-p是设置Memcache监听的端口，我这里设置了12000，最好是1024以上的端口，
#-c选项是最大运行的并发连接数，默认是1024，我这里设置了256，按照你服务器的负载量来设定，
#-P是设置保存Memcache的pid文件，我这里是保存在 /tmp/memcached.pid，
```

## 查看启动的memcache服务：
```
netstat -lp | grep memcached
```

## 查看memcache的进程号（根据进程号，可以结束memcache服务：“kill -9 进程号”）
```
ps -ef | grep memcached
```

## 修改php session支持 memcached
`vi /etc/php.ini`
```
session.save_handler = files
```
改成
```
session.save_handler=memcached
session.save_path="tcp://127.0.0.1:11211"
```

## discuz 开启memcached服务
查找关键字 "memcache"  找到
```
$_config['memory']['memcache']['server'] = '';
```

将其修改为
```
$_config['memory']['memcache']['server'] = '127.0.0.1';
```
