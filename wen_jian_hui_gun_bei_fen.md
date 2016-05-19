# 文件回滚备份

```
<?php 
$shortopts = "v:";
$opt = getopt($shortopts);
if(!isset($opt['v'])) exit("require -v to assign rollback version such as 'v0326' if rollback to master use 'master'");

$rollbackVersion =  trim($opt['v']);
chdir("/App/banyar/");
system("git add .;git commit -m 'checkout v{$rollbackVersion}';git pull;git checkout {$rollbackVersion};");
shell_exec("rsync -av --exclude-from='/App/exclude.list' /App/banyar/* /App/www.banyar.cn");

```