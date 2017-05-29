## 安装
先使用命令查看是否已经安装这个模块：

```
[root@iZ94sbvcp28Z Config]# /usr/local/nginx/sbin/nginx -V
nginx version: nginx/1.10.1
built by gcc 4.4.7 20120313 (Red Hat 4.4.7-11) (GCC)
built with OpenSSL 1.0.1e-fips 11 Feb 2013
TLS SNI support enabled
configure arguments: --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_realip_module --with-http_flv_module --with-http_gzip_static_module --with-http_image_filter_module
```

如果已经安装，会在显示的信息中包含 `--with-http_stub_status_module`信息。如果没有此模块，需要重新安装，编译命令如下：

```
./configure –with-http_stub_status_module
```


## Nginx配置

　　安装后只要修改nginx配置即可，配置如下：

复制代码
```
location /ngx_status {
            stub_status            on;
            access_log             off;
}
```


## 状态查看

配置完成后在浏览器中输入http://127.0.0.1/ngx_status，显示信息如下：

    Active connections: 100 
    server accepts handled requests
     1075 1064 6253 
    Reading: 0 Writing: 5 Waiting: 95 

## 参数说明

　　active connections – 活跃的连接数量

　　server accepts handled requests — 总共处理了107520387个连接 , 成功创建107497834次握手, 总共处理了639121056个请求

　　每个连接有三种状态waiting、reading、writing

　　reading —读取客户端的Header信息数.这个操作只是读取头部信息，读取完后马上进入writing状态，因此时间很短。

　　writing — 响应数据到客户端的Header信息数.这个操作不仅读取头部，还要等待服务响应，因此时间比较长。

　　waiting — 开启keep-alive后等候下一次请求指令的驻留连接.

　　正常情况下waiting数量是比较多的，并不能说明性能差。反而如果reading+writing数量比较多说明服务并发有问题。

 

　　补充：

　　查看Nginx并发进程数：ps -ef | grep nginx | wc -l

　　查看Web服务器TCP连接状态：netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

### 参考文章：

https://nginx.org/en/docs/http/ngx_http_stub_status_module.html#stub_status


