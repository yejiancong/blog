# 性能测试服务PTS

## JDK安装forLinux

查看最新：http://www.oracle.com/technetwork/java/javase/downloads/index.html

下载:jdk-8u131-linux-x64.tar.gz

解压安装:
```
mkdir /usr/local/jdk8
tar zxvf ./jdk-8u131-linux-x64.tar.gz  -C /usr/local/jdk8
```

配置环境变量:

编辑~/.bash_aliases文件

```
export JAVA_HOME=/usr/local/jdk8/jdk1.8.0_131
export CLASSPATH=.:${JAVA_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

保存退出执行以下命令生效：

```
source ~/.bash_aliases
```

在命令行执行 java -version
如果响应一下内容表示安装配置成功
```
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

## Radar下载并安装

下载地址:

https://help.aliyun.com/document_detail/29292.html

```
wget https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/pts/0.3.51/assets/download/radar-for-linux.zip?spm=5176.doc29292.2.2.0xUoxW&file=radar-for-linux.zip
```

配置server.properties文件，修改signature字段等于性能测试用户设置中的用户标识；


chmod +x radar.sh赋予执行权限和./radar.sh start启动Radar

