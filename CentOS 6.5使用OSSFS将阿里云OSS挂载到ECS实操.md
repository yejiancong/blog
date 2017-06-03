OSS是阿里云推出的对象存储服务，和七牛云存储等类似，价格也比较便宜，最近发现一个工具OSSFS可以将OSS挂载到阿里云ECS服务器上，可以达到存储/备份的目的。
OSSFS功能

支持POSIX 文件系统的大部分功能，包括文件读写，目录，链接操作，权限， uid/gid，以及扩展属性（extended attributes）
通过OSS 的multipart 功能上传大文件。
MD5 校验保证数据完整性。
安装

下载其他版本地址: 

https://github.com/aliyun/ossfs/releases

SSH连接到服务器，分别执行下面的命令：

```
wget https://github.com/aliyun/ossfs/releases/download/v1.80.0/ossfs_1.80.0_centos6.5_x86_64.rpm
sudo yum localinstall ossfs_1.80.0_centos6.5_x86_64.rpm
```

安装成功后
```
[root@centos65 ~]# ossfs --version
Aliyun Open Storage Service File System V1.80.0(commit:81f6553) with OpenSSL
Copyright (C) 2010 Randy Rizun <rrizun@gmail.com>
Copyright (C) 2015 Haoran Yang <yangzhuodog1982@gmail.com>
License GPL2: GNU GPL version 2 <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

## 运行与示例

设置bucket name, access key/id信息，将其存放在/etc/passwd-ossfs 文件中， 注意这个文件的权限必须正确设置，建议设为640。


```
echo my-bucket:my-access-key-id:my-access-key-secret > /etc/passwd-ossfs
chmod 640 /etc/passwd-ossfs
```

## 将oss bucket mount到指定目录

```
ossfs my-bucket my-mount-point -ourl=my-oss-endpoint
```

##实际运行

下面是将华南 1 (深圳)bucket名字为test，AccessKeyId是faint， AccessKeySecret是123，oss endpoint是http://oss-cn-hangzhou-internal.aliyuncs.com(内网)挂载到/home/ossfs目录。
Endpoint对照表请访问：OSS开通Region和Endpoint对照表查看https://help.aliyun.com/document_detail/31837.html。

```
echo test:faint:123 > /etc/passwd-ossfs
chmod 640 /etc/passwd-ossfs
mkdir /ossfs/test
ossfs test /ossfs/test -ourl=http://oss-cn-shenzhen-internal.aliyuncs.com -ouid=502 -ogid=501 -oumask=007 -o allow_other
```


取消挂载
直接输入`umount /ossfs/test`即可


## OSSFS权限问题

挂载给用户www,首先查看www id
```
[root@centos56 oss]# id www
uid=502(www) gid=501(www) 组=501(www)
```

开始挂载（-ouid=502 -ogid=501  -oumask=007 -o allow_other 主要解决owncloud提示挂载目录0770的问题）
```
mkdir /ossfs/test
ossfs test /oss/test -ourl=http://oss-cn-shenzhen-internal.aliyuncs.com -ouid=502 -ogid=501 -oumask=007 -o allow_other
```
