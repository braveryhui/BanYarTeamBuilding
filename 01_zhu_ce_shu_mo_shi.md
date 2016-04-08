# 01 注册树模式

什么注册树？
   把对象直接存放的我对象集合里面
使用的场景？
   全局的注册对象，可以通过K-V的方式取出来，不用new 或者 引入文件

###写法
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



