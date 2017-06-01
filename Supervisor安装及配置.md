## Supervisor安装

```shell
# 安装
easy_install supervisor

# 生成默认配置文件
echo_supervisord_conf > /etc/supervisord.conf
mkdir /etc/supervisord.conf.d
```


经过上面两步，Supervisor就安装好了。配置文件模版已经在/etc/supervisord.conf中生成。当然配置文件的位置不一定放在/etc/supervisord.conf中。在不指定配置文件位置的情况下，supervisord和supervisorctl会按照以下三个顺序来搜索配置文件的位置。

- $CWD/supervisord.conf
- $CWD/etc/supervisord.conf
- /etc/supervisord.conf

## 修改配置文件

include区段修改为

```
[include]
files = /etc/supervisord.conf.d/*.conf
```

如需要访问web控制界面，inet_http_server区段修改为

```
[inet_http_server]
port=0.0.0.0:9001
username=username ; 你的用户名
password=password ; 你的密码
```

每个需要管理的进程分别写在一个文件里面，放在/etc/supervisord.conf.d/目录下，便于管理。例如：oos.conf

```
[program:oos]
command=python /data/www/oss-ftp/launcher/start.py
priority=1
numprocs=1
autostart=true
autorestart=true
startretries=10
stopsignal=KILL
stopwaitsecs=10
redirect_stderr=true
```

## 开启自动启动

将supervisord加入系统服务，以下代码来自gist，文件：/etc/init.d/supervisord

```
#!/bin/sh
#
# /etc/rc.d/init.d/supervisord
#
# Supervisor is a client/server system that
# allows its users to monitor and control a
# number of processes on UNIX-like operating
# systems.
#
# chkconfig: - 64 36
# description: Supervisor Server
# processname: supervisord

# Source init functions
. /etc/init.d/functions

RETVAL=0
prog="supervisord"
pidfile="/tmp/supervisord.pid"
lockfile="/var/lock/subsys/supervisord"

start()
{
        echo -n $"Starting $prog: "
        daemon --pidfile $pidfile supervisord -c /etc/supervisord.conf
        RETVAL=$?
        echo
        [ $RETVAL -eq 0 ] && touch ${lockfile}
}

stop()
{
        echo -n $"Shutting down $prog: "
        killproc -p ${pidfile} /usr/bin/supervisord
        RETVAL=$?
        echo
        if [ $RETVAL -eq 0 ] ; then
                rm -f ${lockfile} ${pidfile}
        fi
}

case "$1" in

  start)
    start
  ;;

  stop)
    stop
  ;;

  status)
        status $prog
  ;;

  restart)
    stop
    start
  ;;

  *)
    echo "Usage: $0 {start|stop|restart|status}"
  ;;

esac
```

```
chmod +x /etc/init.d/supervisord
chkconfig supervisord on
service supervisord start

# 查看是否在启动项里
chkconfig --list
```

## Supervisord inet_http_server behind nginx

Supervisord settings:

```
[inet_http_server]
port = 127.0.0.1:9001
```

Nginx settings:
```
server {
		listen       80;
	

		server_name manage.net;
		autoindex on;
		index index.htm index.html index.php;
		root  /data/www/github.com/manage/;
		access_log  /data/nginx_logs/access.manage.net.log;
		
	        auth_basic "Restricted";
       	        auth_basic_user_file /usr/local/nginx/conf/password;
		

		location ~ .*\.(php)?$
		{
			fastcgi_pass   fastcgi_loads_balance;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME   $document_root$fastcgi_script_name;
			include        fastcgi_params;
			client_max_body_size 200m;
		}
	
		location /ngx_status
		{
			stub_status on;
			access_log off;
		}
		
	        location /supervisor/ {
	                proxy_pass http://127.0.0.1:9001/;
                        proxy_http_version 1.1;
		        proxy_buffering     off;
                        proxy_max_temp_file_size 0;
                        proxy_redirect     default;
                        proxy_set_header   Host             $host;
                        proxy_set_header   X-Real-IP        $remote_addr;
                        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
                        proxy_set_header   Connection       "";
	        }

               error_log  /data/nginx_logs/error.manage.net.log;

}
```

## 运行命令



# 启动supervisor
supervisord

# 打开命令行
supervisorctl

# 停止supervisord     
supervisorctl shutdown

# 重新加载配置文件
supervisorctl reload
```

## 注意事项
supervisor不能对守护进程进行监视
