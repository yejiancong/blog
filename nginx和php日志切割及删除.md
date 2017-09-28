`vi spit_logs.php`
```php
<?php
/**
 * Class Log
 * 每天到24点的时候，把当前目录的.log结尾的文件移动到当天的日期文件，同时删除指定日期之前的日志
 */
class Log {
    private $dir;//日志存放目录
    private $day;//日志保留天数
    public function __construct($arr) {
        $this->dir = $arr['dir'];
        $this->day = $arr['day'];

        $this->mvLogFile();
        $this->delLogFile();

    }

    /**
     * 移动日志文件
     */
    private function mvLogFile() {
        if(!$this->dir) {
            throw new Exception('请配置需要备份的日志目录。');
        }

        foreach($this->dir as $v) {
            $bak_dir = $v.'baklog/'.date('Y-m-d').'/';
            if(!is_dir($bak_dir)) {
                mkdir($bak_dir,0777,true);
            }

            $cmd = '\mv '.$v.'*.log '.$bak_dir.' &> /dev/null;kill -USR1 `cat /usr/local/php/var/run/php-fpm.pid` &>/dev/null;kill -USR1 `cat /data/nginx_logs/nginx.pid` &> /dev/null ';
            if($this->execCmd($cmd)) {
                self::writeExpLog('执行命令成功。命令：'.$cmd.' ');
            }

        }

    }

    /**
     * 删除日志文件
     */
    private function delLogFile() {
        //循环出当前目录下的
        foreach($this->dir as $v) {
            $dir = $v.'baklog/';
            $time = mktime(0,0,0,date('m'),date('d')-($this->day),date('Y'));
            $list = scandir($dir);
            foreach($list as $vv) {
                if( is_dir($dirvv = $dir.$vv) && $vv != '.' && $vv != '..' ) {
                        if(strtotime($vv) <= $time) {
                            if($vv && $vv != '/') {
                                $cmd = 'rm -rf '.$dirvv.' ';
                                if($this->execCmd($cmd)) {
                                    self::writeExpLog('执行命令成功。命令：'.$cmd.' ');
                                }
                            }
                        }
                }
            }

        }
    }


    /**
     * @param $cmd
     * @throws Exception
     * 执行命令
     */
    private function execCmd($cmd) {
        exec($cmd,$arr,$status);
        if($status) {
            self::writeExpLog('执行命令 '.$cmd.' 错误。错误代码：'.$status.' ');
            return false;
        } else {
            return true;
        }
    }

    /**
     * 写入异常错误日志
     */
     static public function writeExpLog($msg) {
        $file = '/data/other_logs/spit_log/'.date('Y-m-d').'.log';
        if(!is_dir($dir = dirname($file))) {
            mkdir($dir,0777,true);
        }
        $hd = fopen($file,'a+');
        fwrite($hd,'['.date('Y-m-d H:i:s').'] '.$msg."\n");
        fclose($hd);
    }



}


$arr = array(
    'day'=>2,
    'dir'=>array(
        '/data/php_logs/',
        '/data/nginx_logs/',
    ),
);

try {
    $Log = new Log($arr);
} catch(Exception $e) {
    Log::writeExpLog($e->getMessage());
}

?>
```

`crontab -e`增加加一行每天23时59分执行
```
59 23 * * * /usr/local/php/bin/php -a /root/spit_logs.php > /dev/null
```



