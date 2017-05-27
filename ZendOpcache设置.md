# ZendOpcache设置

## 开启php的zend opcache缓存,php配置追加
```
vi /etc/php.ini
```

```
[opcache]
;扩展地址
zend_extension=/usr/local/php/lib/php/extensions/no-debug-zts-20131226/opcache.so
;开关打开
opcache.enable=1
;开启CLI
opcache.enable_cli=1
;可用内存, 酌情而定, 单位为：Mb
opcache.memory_consumption=528
;Zend Optimizer + 暂存池中字符串的占内存总量.(单位:MB)
opcache.interned_strings_buffer=8
;对多缓存文件限制, 命中率不到 100% 的话, 可以试着提高这个值
opcache.max_accelerated_files=10000
;当这个选项被启用（设置为1），PHP会在opcache.revalidate_freq设置的时间到达后检测文件的时间戳（timestamp）
opcache.validate_timestamps=0
;Opcache 会在一定时间内去检查文件的修改时间, 这里设置检查的时间周期, 默认为 2, 定位为秒
opcache.revalidate_freq=300
;打开快速关闭, 打开这个在PHP Request Shutdown的时候回收内存的速度会提高
opcache.fast_shutdown=1
```

## 设置例外
也许服务器上某些内容，比如正在进行调试的网站等，我们不希望对其进行 OPcache。那就可以通过黑名单来将需要例外的文件排除掉。

在 OPcache 的配置文件中有一行配置，如下，

```
opcache.blacklist_filename=/etc/php.d/opcache*.blacklist
```
该配置指定用于存储文件名黑名单的那个文件。很显然这里使用通配符 * 来指定了一系列文件而不仅仅是特定某个文件。可以一直启用这一行。等到需要排除某些文件的时候，就编辑对应的黑名单文件。例如，针对 `/srv/www/sites/devSite` 文件夹下的所有文件，编辑（或者新建）文件，

```
vim /etc/php.d/opcache-devSite.blacklist
```

内容为，
```
/srv/www/sites/devSite/*
```
嗯，只有这么一行内容。通配符 * 表示所有 devSite 文件夹下的文件。

完了之后重新启动 php-fpm 服务就可以了。

```
sudo /etc/init.d/php-fpm reload
```

### 查看opcache状态：
https://github.com/rlerdorf/opcache-status

https://github.com/amnuts/opcache-gui
