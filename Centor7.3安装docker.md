
首先，查看阿里云ECS Linux服务器系统内核版本：
```
[root@iZwz9g4x9o6sjet97h7prfZ ~]# uname -r
3.10.0-514.16.1.el7.x86_64
```

安装docker
```
[root@iZwz9g4x9o6sjet97h7prfZ ~]# sudo yum -y install docker-io
```

启动docker
```
[root@iZwz9g4x9o6sjet97h7prfZ ~]# sudo service docker start
Redirecting to /bin/systemctl start  docker.service
```

查看版本
```
[root@iZwz9g4x9o6sjet97h7prfZ ~]# docker version
Client:
 Version:         1.12.6
 API version:     1.24
 Package version: docker-1.12.6-28.git1398f24.el7.centos.x86_64
 Go version:      go1.7.4
 Git commit:      1398f24/1.12.6
 Built:           Fri May 26 17:28:18 2017
 OS/Arch:         linux/amd64

Server:
 Version:         1.12.6
 API version:     1.24
 Package version: docker-1.12.6-28.git1398f24.el7.centos.x86_64
 Go version:      go1.7.4
 Git commit:      1398f24/1.12.6
 Built:           Fri May 26 17:28:18 2017
 OS/Arch:         linux/amd64
 ```
 
 开启启动docker
 ```
 sudo chkconfig docker on
 ```
