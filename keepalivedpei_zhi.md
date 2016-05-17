# keepalived配置


```
下载
wget http://www.keepalived.org/software/keepalived-1.2.20.tar.gz
编译安装 
./configure --prefix=/usr/local/keepalived
make
make install

命令设置和开机启动
cp /usr/local/keepalived/sbin/keepalived /usr/sbin/
cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
cp /usr/local/keepalived/etc/rc.d/init.d/keepalived /etc/init.d/
cd /etc/init.d/
chkconfig --add keepalived
chkconfig keepalived on
mkdir -p /etc/keepalived
```

Master: 10.144.130.49
Slave: 10.45.234.101