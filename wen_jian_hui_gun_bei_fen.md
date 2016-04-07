# 文件回滚备份

1.。adsfasdf
2.asdfasdfasdf

```

<?php

define('ROOTPATH',__DIR__);
include ROOTPATH.'/Imooc/AutoLoader.php';

spl_autoload_register("\\Imooc\\AutoLoader::autoLoaderClass");

Imooc\Object::test();
echo '<br />';
APP\Controller\Home\Index::Index();
echo '<br />';
Imooc\Factory::createDataBase();
$db = Imooc\Register::getObj('db');
$db->where('id=123')->order('order by desc')->limit('limit 10');


$prototype = new Imooc\Object();
$prototype::init();
echo "------------<br/ >";
$obj01 = clone $prototype;
$obj02 = clone $prototype;
$obj01::test();
echo '---------<br />';
$obj02::test();

```