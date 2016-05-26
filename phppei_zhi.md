# PHP配置
```
#./configure --prefix=/usr/local/php --with-config-file-scan-dir=/usr/local/php/etc/ --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-iconv-dir=/usr/local/ --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr/ --enable-xml --disable-rpath --enable-safe-mode --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --with-curlwrappers --enable-mbregex --enable-fpm --enable-mbstring  --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-ldap --with-ldap-sasl --with-xmlrpc --enable-zip --enable-soap --with-gettext --enable-opcache --enable-session --without-pear --with-iconv

#vi MakeFile 新加 －llber 否则make会出错
#make 
#make install
```

```
# cp /home/installSoftWare/php-7.0.3/sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf
```
补充：需要重新安装下pdo_mysql扩展在php-7.0.3/ext目录下 
###其他扩展配置 
extension=swoole.so
extension=redis.so
extension=seaslog.so
extension=inotify.so
extension=pdo_mysql.so
extension=yar.so
;[seaslog] congigure 2016 0227
seaslog.default_basepath = /var/log/seaslog     ;默认log根目录
seaslog.default_logger = default                ;默认logger目录
seaslog.disting_type = 1                        ;是否以type分文件 1是 0否(默认)
seaslog.disting_by_hour = 1                     ;是否每小时划分一个文件 1是 0否(默认)
seaslog.use_buffer = 1                          ;是否启用buffer 1是 0否(默认)
seaslog.buffer_size = 100                       ;buffer中缓冲数量 默认0(不使用buffer_size)
seaslog.level = 0                               ;记录日志级别 默认0(所有日志)
seaslog.trace_error = 1                         ;自动记录错误 默认1(开启)
seaslog.trace_exception = 0                     ;自动记录异常信息 默认0(关闭)
;end seaslog configure 2016 0227

;[opcache]
zend_extension=opcache.so
opcache.memory_consumption=64
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.force_restart_timeout=180
opcache.revalidate_freq=60
opcache.fast_shutdown=1
opcache.enable_cli=1

###安全配置 
safe_mode = off   ＃开启的话php 可以执行一下系统函数，建议关闭 （可搜索受此函数影响的php函数）
如果只需要配置某一个目录可以执行则 设置为on并指定 safe_mode_exec_dir=string 目录来执行系统函数
disable_function = systme exex phpinfo等等 #进展系统函数和高危函数
register_globas=off                #关闭全局变量 PHP在进程启动时,会根据register_globals的设置，
                                    判断是否将$_GET、$_POST、$_COOKIE、$_ENV、$_SERVER、
                                    $REQUEST等数组变量里的内容自动注册为全局变量
expose_php = off                   #禁止暴露php版本号
magic_quotes_pc ＝ on               #防止sql注入 本质是把 ‘ 转为／
display_errors = off                #不显示到前端错误 
error_reporting =                   #日志显示的级别
display_startup_errors =Off       ＃ php启动时产生的错误由此选项进行控制allow_url_include =Off                                         ＃PHP通过此选项控制是否允许通过include/require来执行一个远程文件