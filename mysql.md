# Mysql安装

###安装Mysql
```
编译配置
useradd mysql
groupadd mysql

wget  http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.25.tar.gz
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql\
-DMYSQL_DATADIR=/App/banyardata\
-DMYSQL_UNIX_ADDR=/var/mysql/mysqld.sock\
 -DDEFAULT_CHARSET=utf8\
 -DWITH_EXTRA_CHARSETS=all\
 -DDEFAULT_COLLATION=utf8_general_ci\
 -DWITH_INNOBASE_STORAGE_ENGINE=1\
 -DWITH_MYISAM_STORAGE_ENGINE=1\
 -DWITH_SPHINX_STORAGE_ENGINE=1\
 -DWITH_INNOBASE_STORAGE_ENGINE=1\
 -DWITH_MEMORY_STORAGE_ENGINE=1\
 -DMYSQL_USER=mysql
  make & make install 
```

###主要文件目录
```
/etc/my.cnf    配置文件位置
/usr/local/mysql/bin/  bin目录位置
/var/log/mariadb/mariadb.log 日志位置 
/usr/local/mysql/bin/mysqld_safe &  启动脚本
```

###设置root密码
```
设置root密码
# mysqld_safe --user=mysql --skip-grant-tables --skip-networking & 
# mysql -u root mysql 
mysql> UPDATE user SET Password=PASSWORD('passwd') where USER='root'; 
mysql> FLUSH PRIVILEGES; 
mysql> quit 
＃如果提示Can't connect to local MySQL server through socket 
#./mysql -u root -S /var/mysql/mysqld.sock 指定下socket位置
```