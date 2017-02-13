# Monit 安装配置 

##1.安装Monit
  下载地址 https://mmonit.com/monit/  需要下载源码
  yum安装需要装依赖 
 - yum install epel*
 - yum install pam-devel
 - yum insall pam
 - yum install monit
  
  
###源码安装 
./configure 错误 
configure: error: PAM enabled but headers or library not found, install the PAM development support or run configure --without-pam
  yum install pam-devel
  yum install pam  即可解决

make 
make install 

##2.配置 
mv monitrc /etc/monitrc
chown root:root /etc/monitrc
chmod 0700 /etc/monitrc

 monit -t 检查 配置文件是否正确 
 New Monit id: 0414eed13ff8027c154d0555e9f7f97f
 Stored in '/root/.monit.id'

monit   
monit start nginx 
monit stop all /nginx 


3.配置web访问 
set httpd port 2812 and
    #use address all     # only accept connection from localhost
    allow 0.0.0.0/0.0.0.0 #允许ip所有访问
    allow admin:monit      # 访问用户名和密码


4.配置监控进程
#vi  /etc/monitrc 编辑配置文件结尾配置监控目录
   include /etc/monit.d/* 
#mkdir /etc/monit.d/ #新建监控服务的目录 默认的


#cd /etc/monit.d/  
#vi nginx  #编辑添加监控nginx 如果被关掉重新启动,添加一下代码
  check process  nginx with pidfile /run/nginx.pid
  start program = "/usr/local/nginx/sbin/nginx " with timeout 10 seconds
  stop program  = "/usr/local/nginx/sbin/nginx -s stop"
#vi swoole  #编辑添加监控swoole是否挂掉，如果挂掉重启
check process swoole_server with MATCHING swoole_server
start program = "/usr/local/php/bin/php /App/www.banyar.cn/swoole/banyarWebServer.php" with timeo


#vi swoole  #编辑添加监控swoole是否挂掉，如果挂掉重启
check process swoole_server with MATCHING swoole_server
start program = "/usr/local/php/bin/php /App/www.banyar.cn/swoole/banyarWebServer.php" with timeout 10 seconds