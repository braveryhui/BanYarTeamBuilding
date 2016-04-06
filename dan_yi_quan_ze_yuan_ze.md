# 单一权责原则
```
<?php
header("Content-type: text/html; charset=utf-8");
phpinfo();

$shortopts = "v:";
$opt = getopt($shortopts);
if(!isset($opt['v'])) exit("require -v to assign rollback version");

$rollbackVersion =  trim($opt['v']);
chdir("/App/gitHub/test");
system("git add .;git commit -m 'checkout v{$rollbackVersion}';git pull;git checkout {$rollbackVersion};");
shell_exec("rsync -avz --delete /App/gitHub/test/ /App/gitHub/testbak/");
```

