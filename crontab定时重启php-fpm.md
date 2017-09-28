`vi restart-php-fpm.sh`
```
#!/bin/sh
/etc/init.d/php-fpm restart
echo "restart php-fpm"
echo `date +%F....%T` >> /data/phpkill_logs/restart.log
```

脚本权限设置
```
chmod +x restart-php-fpm.sh
```

`crontab -e`增加一行每天1点01分重启php-fpm
```
01 1 * * * /root/restart-php-fpm.sh > /dev/null
```
