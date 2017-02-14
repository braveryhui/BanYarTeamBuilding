# Monit服务器监控

标签： Linux

---


Monit是一个跨平台的用来监控Unix/linux系统（比如Linux、BSD、OSX、Solaris）的工具。Monit特别易于安装，而且非常轻量级（只有500KB大小），并且不依赖任何第三方程序、插件或者库。然而，Monit可以胜任全面监控、进程状态监控、文件系统变动监控、邮件通知和对核心服务的自定义动作等场景。易于安装、轻量级的实现以及强大的功能，让Monit成为一个理想的后备监控工具。

官网：https://mmonit.com/
文档：https://mmonit.com/monit/documentation/monit.html

## 安装
```
yum install monit
```

当前使用版本：
``` c
# monit -V
This is Monit version 5.17.1
Built with ssl, with pam and with large files
Copyright (C) 2001-2016 Tildeslash Ltd. All Rights Reserved.
```

## 常用命令
``` 
monit -t # 配置文件检测
monit # 启动monit daemon
monit -c /var/monit/monitrc # 启动monit daemon时指定配置文件
monit reload # 当更新了配置文件需要重载
monit status # 查看所有服务状态
monit status nginx # 查看nginx服务状态
monit stop all # 停止所有服务
monit stop nginx # 停止nginx服务
monit start all # 启动所有服务
monit start nginx # 启动nginx服务
monit -V # 查看版本
```

## 配置文件
使用yum安装默认配置文件在：
```
/etc/monitrc # 主配置文件
/etc/monit.d/ # 单独配置各项服务
```

为了保护控制文件和密码的安全性，`monitrc`必须具有读写权限不超过`0700`(u=xrw,g=,o=)。

主配置文件主要配置全局：
/etc/monitrc
``` c
## Global section
set daemon  30

set logfile syslog

# 邮箱设置
set mailserver xxx@xxx
    username "xxx" password "xxx"
   # using ssl
   
set alert xxx@xxx
set alert xxx@xxx #可以设置多个

set mail-format {
  from: xxx@xxx
  subject: [$SERVICE] $EVENT
  message:
[$SERVICE] $EVENT

  Date:         $DATE
  Action:         $ACTION
  Host:         $HOST
  Description: $DESCRIPTION

Your faithful employee,
Monit }

# 设置web服务认证
set httpd port 2812 and
    # ssl enable
    # pemfile /etc/certs/monit.pem
    # use address all  # only accept connection from localhost
    allow 127.0.0.1       # 允许localhost连接
    allow admin:monit      # require user 'admin' with password 'monit'
    
## Services

## Includes
include /etc/monit.d/*
```


配置文件关键字：
'if', 'and', 'with(in)', 'has', 'us(ing|e)', 'on(ly)', 'then', 'for', 'of' 。

## 如何监控

### 基本流程

1.修改主配置文件
2.在`/etc/monit.d/`增加指定服务的配置文件。配置变写完毕，使用下列，命令检测是否正确：
```
monit -t
```
3.启动monit:
```
monit
```
4.启动所有服务或者单个服务：
```
monit start all
```
5.若修改了配置文件，重载配置：
```
monit reload
```

### 设置错误提醒
Monit默认情况下如果一个服务失败只发送一个通知：
```
alert foo@bar
```

如果您希望在服务保持处于失败状态时每十个周期通知一次，您可以使用：
```
alert foo@bar with reminder on 10 cycles
```
同样，如果您想在每个失败的周期获得通知，您可以使用：
```
alert foo@bar with reminder on 1 cycle
```

要禁止某些用户和服务的警报，可以在服务检查的局部配置里添加语句：
```
noalert mail-address
```

## 服务类型
首先需要理解在monit里什么是服务(service)。看监控语法：
```
check <类型> <服务名> [PATH <path>] [ADDRESS <host address>]
```
其中类型是monit支持的监控类型，一共有：system、file、process、fifo、filesystem、directory、host、network、program。
服务名必需是英文且唯一，不可以出现重复！
后面的带`[]`是根据类型需要添加的。

## 服务类型语法
每个服务条目由关键字组成`check`，后面是服务类型。每个条目需要唯一的描述性名称，可以自由选择。此名称由Monit用于在内部和与用户的所有交互中引用该服务。

目前，支持九种类型的检查语句：

### 进程
```
CHECK PROCESS <unique name> <PIDFILE <path> | MATCHING <regex>>
```
`<path>`是程序的pid文件的绝对路径。pid文件是一个包含进程唯一ID的文件。如果pid文件不存在或不包含正在运行的进程的PID编号，则Monit将调用该条目的start方法（如果已定义）。

`<regex>`是使用PID文件的替代方法，并使用进程名称模式匹配来查找要监视的进程。选择具有最长正常运行时间的最顶部匹配的父级，因此如果进程名称是唯一的，则此检查形式是最有用的。应该尽可能使用Pid文件，因为它定义了预期的PID。您可以测试一个进程是否匹配来自命令行使用的模式monit procmatch "regex-pattern"。这将列出匹配或不匹配的所有进程，regex模式。

### 文件
```
CHECK FILE <unique name> PATH <path>
```
`<path>`是文件的绝对路径。如果文件不存在，Monit将调用该条目的start方法（如果已定义），如果`<path>`不指向常规文件类型（例如目录），Monit将禁用此条目的监视。如果Monit在被动模式下运行或者没有定义start方法，Monit只会在错误时发送警报。

### Fifo
```
CHECK FIFO <unique name> PATH <path>
```
`<path>`是fifo的绝对路径。如果fifo不存在，Monit将定义调用该条目的start方法，如果`<path>`没有指向fifo类型（例如目录），Monit将禁用对该条目的监视。如果Monit在被动模式下运行或者没有定义start方法，Monit只会在错误时发送警报。

### 文件系统
```
CHECK FILESYSTEM <unique name> PATH <path>
```
`<path>`是设备/磁盘，安装点，文件或作为文件系统一部分的目录的路径。建议直接使用块特殊文件（例如Linux上的`/dev/hda1`或Solaris上的`/dev/dsk/c0t0d0s1`等）如果使用挂载点（例如`/data`），请注意文件系统是卸载的测试仍然是真的，因为挂载点存在。

如果文件系统不可用，Monit将调用该条目的start方法（如果已定义）。如果<path>不指向文件系统，Monit将禁用对此条目的监视。如果Monit在被动模式下运行或者没有定义start方法，Monit只会在错误时发送警报。

### 目录
```
CHECK DIRECTORY <unique name> PATH <path>
```
`<path>`是目录的绝对路径。如果目录不存在，Monit将调用该条目的start方法（如果已定义）。如果`<path>`不指向目录，monit将禁用对此条目的监视。如果Monit在被动模式下运行或者没有定义启动方法，Monit只会在错误时发送警报。

### 远程主机
```
CHECK HOST <unique name> ADDRESS <host address>
```
主机地址可以指定为主机名字符串或点分十进制格式的IP地址字符串。例如，tildeslash.com或“64.87.72.95”。

### 系统
```
CHECK SYSTEM <unique name>
```
的唯一的名称通常是本地主机名，而是可以使用任何描述性名称。如果使用变量$ HOST作为名称，它将扩展为主机名。此检查允许监控一般系统资源，如CPU使用率，总内存使用或负载平均。该唯一名称在邮件警报中用作系统主机名，在M/Monit中用作主机条目的初始名称。

### 自定义
```
CHECK PROGRAM <unique name> PATH <executable file> [TIMEOUT <number> SECONDS]
```
`<path>`是可执行程序或脚本的绝对路径。该状态测试允许一个检查程序的退出状态。如果程序没有在`<number>`秒内完成执行，Monit将终止它。默认程序超时为300秒（5分钟）。程序的输出被记录并在用户界面和警报中可用，默认情况下最大为512B。您可以使用set limits语句自定义限制。

### 网络
```
CHECK NETWORK <unique name> <ADDRESS <ipaddress> | INTERFACE <name>>
```
`<ipaddress>`是受监视网络接口的IPv4或IPv6地址。也可以在Linux上使用接口名称，例如“eth0”。



## 服务检测时间
可以使用every语句修改服务检查计划。

有三种变体：

1.轮询周期倍数
```
EVERY [number] CYCLES
```
2.Cron-style
```
EVERY [cron]

# [cron]
# *  *  *  *  *
# 分 时 日 月 周
```
3.与Cron-style相反（do-not-check）
```
NOT EVERY [cron]
```

示例：
示例1：每两个周期检查一次
```
check process nginx with pidfile /var/run/nginx.pid
       every 2 cycles
```
示例2：在上午8点到下午7点之间检查每个工作日
```
check program checkOracleDatabase
        with path /var/monit/programs/checkoracle.pl
       every "* 8-19 * * 1-5"
```
示例3：在星期日0AM到3AM之间不要在备份窗口中运行检查，否则运行具有常规轮询周期频率的检查。
```
check process mysqld with pidfile /var/run/mysqld.pid
       not every "* 0-3 * * 0"
```
注意不要使用特定的分钟，因为Monit可能不会在那分钟运行。

## 服务重启限制
Monit提供了一种重启限制机制，用于服务在较长时间内拒绝启动或响应的情况。
超时语句的语法如下（关键字在大写）：
``` c
IF <number> RESTART <number> CYCLE(S) THEN <action>
```
该行动值是常见的任何一个动作或超时（为向后兼容，等于取消监视行动）。

下面是一个示例，如果Monit将在3个周期内重新启动服务2次，将取消监视服务：
``` c
if 2 restarts within 3 cycles then unmonitor
```
要在禁用监视后使Monit再次检查服务，请从命令行运行monit monitor servicename。

超时设置自定义exec的示例：
``` c
if 5 restarts within 5 cycles then exec "/foo/bar"
```
停止服务的示例：
``` c
if 7 restarts within 10 cycles then stop
```

## 服务示例
一个完整的HOST监控服务语法：
``` c
check host <service> address <address or ip>
  if failed
        xxx
  then alert
  alert xx@xxx
```
解释：
第一行是检查类型为host的服务，需要设定服务名及服务器地址；
第二行至第四行的意思是中间的预期代码`xxx`如果失败，则执行`then alert`；
最后一行`alert xx@xxx`配置局部推送的邮箱，可选。可以多行，表示配置多个。

第二行至第四行也可以写成一行：
```
if failed xxx then alert
```

下面是示例：

``` c
## system
check system $HOST
   if loadavg (1min) > 4 then alert
   if loadavg (5min) > 2 then alert
   if cpu usage > 95% for 10 cycles then alert
   if memory usage > 75% then alert
   if swap usage > 25% then alert

## file
check file apache_bin with path /usr/local/apache/bin/httpd
   if failed checksum and 
      expect the sum 8f7f419955cefa0b33a2ba316cba3659 then unmonitor
   if failed permission 755 then unmonitor
   if failed uid root then unmonitor
   if failed gid root then unmonitor
   alert security@foo.bar on {
          checksum, permission, uid, gid, unmonitor
       } with the mail-format { subject: Alarm! }
   group server

## process
check process apache with pidfile /usr/local/apache/logs/httpd.pid
   start program = "/etc/init.d/httpd start" with timeout 60 seconds
   stop program  = "/etc/init.d/httpd stop"
   if cpu > 60% for 2 cycles then alert
   if cpu > 80% for 5 cycles then restart
   if totalmem > 200.0 MB for 5 cycles then restart
   if children > 250 then restart
   if loadavg(5min) greater than 10 for 8 cycles then stop
   if failed host www.tildeslash.com port 80 protocol http 
      and request "/somefile.html"
   then restart
   if failed port 443 protocol https with timeout 15 seconds then restart
   if 3 restarts within 5 cycles then unmonitor
   depends on apache_bin
   group server

## filesystem
check filesystem datafs with path /dev/sdb1
	start program  = "/bin/mount /data"
	stop program  = "/bin/umount /data"
	if failed permission 660 then unmonitor
	if failed uid root then unmonitor
	if failed gid disk then unmonitor
	if space usage > 80% for 5 times within 15 cycles then alert
	if space usage > 99% then stop
	if inode usage > 30000 then alert
	if inode usage > 99% then stop
	group server

## file's timestamp
check file database with path /data/mydatabase.db
   if failed permission 700 then alert
   if failed uid data then alert
   if failed gid data then alert
   if timestamp > 15 minutes then alert
   if size > 100 MB then exec "/my/cleanup/script" as uid dba and gid dba
   
## directory permission
check directory bin with path /bin
   if failed permission 755 then unmonitor
   if failed uid 0 then unmonitor
   if failed gid 0 then unmonitor
   
## remote host
check host myserver with address 192.168.1.1
   if failed ping then alert
   if failed port 3306 protocol mysql with timeout 15 seconds then alert
   if failed port 80 protocol http
      and request /some/path with content = "a string"
   then alert
   
## network link status
check network public with interface eth0
   if failed link then alert
   if changed link then alert
   if saturation > 90% then alert
   if download > 10 MB/s then alert
   if total upload > 1 GB in last hour then alert
   
## custom program status output
check program myscript with path /usr/local/bin/myscript.sh
   if status != 0 then alert

```


## 实践
### 监听Nginx、php-fpm及API接口
``` c
# check nginx process
check process nginx with pidfile /run/nginx.pid
  start program = "/usr/local/nginx/sbin/nginx " with timeout 10 seconds
  stop program  = "/usr/local/nginx/sbin/nginx -s stop"
  if changed pid then restart

# check php-fpm process
check process php-fpm with MATCHING  php-fpm
 start program = "/usr/local/php/sbin/php-fpm" with timeout 10 seconds
 stop program  = "/usr/bin/killall php-fpm" with timeout 10 seconds
 if failed port 9000 for 3 cycles then restart

# check http status
check host dev_xxx_http address xxx
  start program = "/usr/local/php/sbin/php-fpm ; /usr/local/nginx/sbin/nginx -s reload" with timeout 10 seconds
  stop program  = "/usr/bin/killall php-fpm ; /usr/local/nginx/sbin/nginx -s stop" with timeout 10 seconds
  if failed
        port 80
        protocol http
        and status = 200
        for 3 cycles
  then restart
  #alert xxx@xxx #可以单独设置新的通知者
  #alert xxx@xxx

  if failed
        port 80
        protocol http
        request "/Api/Login/Get_Userinfo/"
        and status = 200
        for 3 cycles
  then restart
```

### 监听TCP
``` c
check host dev_xxx_swoole_xxx address xxx
  start program = "/usr/local/php/bin/php Server.php" with timeout 10 seconds
  stop program  = "/usr/bin/kill -9 $(ps -aux|grep -E 'Server|swoole_server'|grep -v grep|awk '{print $2}')" with timeout 10 seconds
  if failed port xxx type tcp for 3 cycles then restart
```





