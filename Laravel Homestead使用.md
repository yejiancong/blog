
## 安装

下载地址http://vagrantup.com

## 添加Vagrant箱子

```
vagrant box add laravel/homestead
```

## 安装 Homestead

一旦箱子被添加到Vagrant安装目录下，你就可以通过 Composer 的 global 指令来安装 Homestead 命令行工具了：

```
composer global require "laravel/homestead=~2.0"
```


一旦安装了 Homestead 命令行工具，请执行 init 来创建 Homestead.yaml 配置文件：

```
homestead init
```

生成的 Homestead.yaml 文件将被放置于 ~/.homestead 目录下。如果你使用的是 Mac 或 Linux 操作系统，还可以通过执行 homestead edit 指令来编辑 Homestead.yaml 文件：

```
homestead edit
```

## 连接到数据库


“homestead”数据库是为箱子外面的MySQL和Postres配置的。为了更加方便，Laravel的本地数据库配置默认设置为使用这个数据库。

想通过你主机上的Navicat或Sequel Pro连接MySQL或Postgres，你应该使用端口33060（MySQL）或54320（Postgres）来连接“127.0.0.1”。这两个数据库的用户名和密码都是“homestead” / “secret”。
