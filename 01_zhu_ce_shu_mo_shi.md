# 01 注册树模式

###什么注册树？

>    把对象直接存放的我对象集合里面

###使用的场景？
 
>   全局的注册对象，可以通过K-V的方式取出来，不用new 或者 引入文件


###PHP写法
####实例01
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


Register::set('db',$db);  #注册进全局树
db = Imooc\Register::getObj('db'); ＃根据K-V取出对象
```

####实例02
利用静态方法更方便的存取数据，因为静态方法是直接放到内存中的，用静态方法存取对象可以提高对象的访问效率，当然也会造成内存的消耗
```
<?php
/**
* Registry class
*/
class Package {
 
    protected static $data = array();
 
    public static function set($key, $value) {
        self::$data[$key] = $value;
    }
 
    public static function get($key) {
        return isset(self::$data[$key]) ?     self::$data[$key] : null;
    }
 
    final public static function removeObject($key) {
        if (array_key_exists($key, self::$data)) {
            unset(self::$data[$key]);
        }
    }
}
 
 
Package::set('name', 'Package name');
 
print_r(Package::get('name'));
```
####实力03 注册树＋工厂
和工厂结合保存一个不同初始化对象的状态 
```
<?php
 
class Product {
 
    protected $id;
 
    public function __construct($id) {
        $this->id = $id;
    }
 
    public function getId() {
        return $this->id;
    }
}
 
class Factory {
 
    protected static $products = array();
 
    public static function pushProduct(Product $product) {
        self::$products[$product->getId()] = $product;
    }
 
    public static function getProduct($id) {
        return isset(self::$products[$id]) ? self::$products[$id] : null;
    }
 
    public static function removeProduct($id) {
        if (array_key_exists($id, self::$products)) {
            unset(self::$products[$id]);
        }
    }
}
 
 
Factory::pushProduct(new Product('first'));
Factory::pushProduct(new Product('second'));
 
print_r(Factory::getProduct('first')->getId());
// first
print_r(Factory::getProduct('second')->getId());
// second

```

参考：
[PHP常用设计模式](http://www.admin10000.com/document/7115.html)

[gitHub上php设计模式](https://github.com/domnikl/DesignPatternsPHP)



