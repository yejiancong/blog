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

### 查看opcache状态：
https://github.com/rlerdorf/opcache-status
https://github.com/amnuts/opcache-gui
