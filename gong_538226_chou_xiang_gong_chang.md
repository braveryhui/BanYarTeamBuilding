# 工厂&抽象工厂

###什么是工厂
    对象的实例化创建，隐藏不必要的细节，快速创建对象
  
  
###应用场景
  1. 创建单例对象 创建各种对象
  2. 创建常用Model
  3. 构建常用操作

###PHP写法


#####工厂
```
<?php
 
interface Factory {
    public function getProduct();
}
 
interface Product {
    public function getName();
}
 
class FirstFactory implements Factory {
 
    public function getProduct() {
        return new FirstProduct();
    }
}
 
class SecondFactory implements Factory {
 
    public function getProduct() {
        return new SecondProduct();
    }
}
 
class FirstProduct implements Product {
 
    public function getName() {
        return 'The first product';
    }
}
 
class SecondProduct implements Product {
 
    public function getName() {
        return 'Second product';
    }
}
 
$factory = new FirstFactory();
$firstProduct = $factory->getProduct();
$factory = new SecondFactory();
$secondProduct = $factory->getProduct();
 
print_r($firstProduct->getName());
// The first product
print_r($secondProduct->getName());
// Second product
```

#####抽象工厂
```
在很多情况下，需要为系统中的多个类创建单例的构造方式，这样，可以建立一个通用的抽象父工厂方法：
<?php
 
abstract class FactoryAbstract {
 
    protected static $instances = array();
 
    public static function getInstance() {
        $className = static::getClassName();
        if (!(self::$instances[$className] instanceof $className)) {
            self::$instances[$className] = new $className();
        }
        return self::$instances[$className];
    }
 
    public static function removeInstance() {
        $className = static::getClassName();
        if (array_key_exists($className, self::$instances)) {
            unset(self::$instances[$className]);
        }
    }
 
    final protected static function getClassName() {
        return get_called_class();
    }
 
    protected function __construct() { }
 
    final protected function __clone() { }
}
 
abstract class Factory extends FactoryAbstract {
 
    final public static function getInstance() {
        return parent::getInstance();
    }
 
    final public static function removeInstance() {
        parent::removeInstance();
    }
}
// using:
 
class FirstProduct extends Factory {
    public $a = [];
}
class SecondProduct extends FirstProduct {
}
 
FirstProduct::getInstance()->a[] = 1;
SecondProduct::getInstance()->a[] = 2;
FirstProduct::getInstance()->a[] = 3;
SecondProduct::getInstance()->a[] = 4;
 
print_r(FirstProduct::getInstance()->a);
// array(1, 3)
print_r(SecondProduct::getInstance()->a);
// array(2, 4)
```

 


