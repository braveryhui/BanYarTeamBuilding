# 03-02 单例

###什么是单例？

> 通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源，也可以有多个实例MutiTon




###使用场景

>    数据库全局只返回一个实例
   IOS：UserDefualts,UIApplication 等
   Android:sharePerference 等


###核心点

> 1.复用 （全局唯一 也可以有多个实例）
2.防止外部调用构造方法
3.防止被clone
4.防止被反序列化


###延伸


>   1.重写后子类覆盖父类
  2.重写可以重新定义成员方法权限，比如父类的是Public子类可以重新生定义为Private
  3.不允许序列化，序列化是把对象状态保存下来，需要时可以恢复使用
  4.java和php 都是用序列化来保存对象的



###PHP写法

```

<?php
namespace Banyar;
class Singleton
{

    private static $instances = null;

    /**
     * 重写构造方案 为private 防止被外部类调用: private!
     */
    private function __construct()
    {

    }
    /**
     * 获取单例对象
     */
    public static function getInstance($instanceName)
    {
        if (!self::$instance) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    /**
     * 防止被clone
     */
    private function __clone()
    {
    }
    /**
     * 防止反序列化
     */
    private function __wakeup()
    {
    }
}

 ?>

```
多数实际的应用场景会是抽象工厂和单例的结合使用，TP是用的创建单例的公共方法，

```
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