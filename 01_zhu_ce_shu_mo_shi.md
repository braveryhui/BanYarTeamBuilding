# 01 注册树模式

什么注册树？
   把对象直接存放的我对象集合里面
使用的场景？
   全局的注册对象，可以通过K-V的方式取出来，不用new 或者 引入文件

```
<?php
namespace Imooc;
class Register
{
  protected static $objTree;
  static function set($alias,$obj)
  {
      self::$objTree[$alias] = $obj;
  }
  static function getObj($name)
  {
    return self::$objTree[$name];
  }
  static function _unset($name)
  {
     unset(self::$objTree[$name]);
  }
}

```

```
<?php
namespace Imooc;
class Register
{
  protected static $objTree;
  static function set($alias,$obj)
  {
      self::$objTree[$alias] = $obj;
  }
  static function getObj($name)
  {
    return self::$objTree[$name];
  }
  static function _unset($name)
  {
     unset(self::$objTree[$name]);
  }
}

```


1.下载gitbook 
2.新建github同步库 并且是公开的
3.gitbook上配置 github的库并授权 
4.gitbook－cli 生成pdf和epub mobi的电子书格式 
5.
