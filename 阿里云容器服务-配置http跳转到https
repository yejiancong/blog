# 阿里云容器服务-配置http跳转到https


## 容器在不暴露端口的情况下进行http跳转到https

```
server {
    listen       80;
    server_name  localhost;
    root  /data/www/public;


    # http跳转https
    # 负载均衡需要在443端口开启SLB监听协议
    if ($http_x_forwarded_proto != "https") {
      return 301 https://$server_name$request_uri;
    }

    
    location / {
        index  index.html index.htm index.php;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
            expires 30d;
    }

    location ~ .*\.(js|css)?$ {
            expires 30d;
    }

   location ~ \.php$ {
       fastcgi_pass   127.0.0.1:9000;
       fastcgi_index  index.php;
       fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
       include        fastcgi_params;
   }

}
```

## 好处

- ECS服务不用暴露端口,增加ECS的复用性
- 负载均衡的80端口直接用四层协议,流量也可以用简单路由进行分发

## 前提条件

负载均衡需要在443端口开启SLB监听协议
