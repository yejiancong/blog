# SLB负载均衡HTTPS监听配置

在SLB中添加SSL协议，这样SLB和用户之间使用HTTPS，SLB和服务器之间使用HTTP。这样转移到HTTPS的成本是最低的


## 创建证书
私钥是在Linux中通过下面的openssl命令生成的：

```
openssl req -new -newkey rsa:2048 -nodes -keyout my.key -out my.csr
```

证书是通过my.csr在godaddy上生成的。


申请godaddy 购买的豪华版通配符 SSL 证书成功后,

下载后有
`c5666cc3bacd253c.crt`和`gd_bundle-g2-g1.crt`文件


在阿里云SLB>负载均衡>证书管理>创建服务器证书
```
openssl rsa -in my.key -out my.pem
```

证书内容填写`c5666cc3bacd253c.crt`的内容,

私钥内容填写`my.pem`的内容

## 配置HTTPS单向认证

阿里云用户指南:负载均衡 > 用户指南 > 配置HTTPS监听 > 配置HTTPS单向认证

https://help.aliyun.com/document_detail/54523.html
