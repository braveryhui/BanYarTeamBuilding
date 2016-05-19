# PHP配置
```
#./configure --prefix=/usr/local/php --with-config-file-scan-dir=/usr/local/php/etc/ --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-iconv-dir=/usr/local/ --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr/ --enable-xml --disable-rpath --enable-safe-mode --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --with-curlwrappers --enable-mbregex --enable-fpm --enable-mbstring  --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-ldap --with-ldap-sasl --with-xmlrpc --enable-zip --enable-soap --with-gettext --enable-opcache --enable-session --without-pear --with-iconv

#vi MakeFile 新加 －llber 
#make 
#make install
```
```
# cp /home/installSoftWare/php-7.0.3/sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf
```