# 03-02 单例

###什么是单例？

> 通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源，也可以有多个实例MutiTon，如果可以用多个单例，需要结合注册树来搞定 
> 
> 多数Web端和APP端的都会把单例做成一个抽象工厂（Thinkphp的get_instance_of的全局方法）或者宏定义（IOS & Android）

###使用场景

>    数据库全局只返回一个实例
   IOS：UserDefualts,UIApplication 等
   Android:sharePerference 等
   PHP:数据库连接 全局注册配置等 
   


###核心点


> 1. 复用 （全局唯一 也可以有多个单实例注册到注册树上）
2. 防止外部调用构造方法
3. 防止被clone
4. 防止被反序列化
5. 线程安全的问题 
6. 不会被销毁

###延伸

> 1. 重写后子类覆盖父类
2. 重写可以重新定义成员方法权限，比如父类的是Public子类可以重新生定义为Private
3. 不允许序列化，序列化是把对象状态保存下来，需要时可以恢复使用
4. java和php 都是用序列化来保存对象的



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
####多数实际的应用场景会是抽象工厂和单例的结合使用，TP是用的创建单例的公共方法，


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

#####Thinkphp common文件的functions.php中获取单例的方法

```
/**
 * 取得对象实例 支持调用类的静态方法
 * @param string $name 类名
 * @param string $method 方法名，如果为空则返回实例化对象
 * @param array $args 调用参数
 * @return object
 */
function get_instance_of($name, $method='', $args=array()) {
    static $_instance = array();
    $identify = empty($args) ? $name . $method : $name . $method . to_guid_string($args);
    if (!isset($_instance[$identify])) {
        if (class_exists($name)) {
            $o = new $name();
            if (method_exists($o, $method)) {
                if (!empty($args)) {
                    $_instance[$identify] = call_user_func_array(array(&$o, $method), $args);
                } else {
                    $_instance[$identify] = $o->$method();
                }
            }
            else
                $_instance[$identify] = $o;
        }
        else
            halt(L('_CLASS_NOT_EXIST_') . ':' . $name);
    }
    return $_instance[$identify];
}
```

###IOS写法
 
单例怎么解决线程的 不安全呢?
 @synchronized 来创建互斥锁即可
```

/*********
 1.常规写法
 *********/
// 获取一个 sharedInstance 实例,如果有必要的话,实例化一个
  这个是实例化的线程安全并不是调用的时候线程安全 ，IOS里面处理单例的线程安全 GCD+
 
+ (BVNonARCSingleton *)sharedInstance {
    if (sharedInstance == nil) {
        sharedInstance = [[super allocWithZone:NULL] init];
    }
    return sharedInstance;
}
// 当第一次使用这个单例时,会调用这个 init 方法。
- (id)init
{
    self = [super init];
    if (self) {
        // 通常在这里做一些相关的初始化任务
    }
    return self;
}
// 这个 dealloc 方法永远都不会被调用--因为在程序的生命周期内容,该单例一直都存在。(所以该方法可以不 用实现)
-(void)dealloc
{
    [super dealloc];
}
// 通过返回当前的 sharedInstance 实例,就能防止实例化一个新的对象。
+ (id)allocWithZone:(NSZone*)zone {
    return [[self sharedInstance] retain];
}

// 同样,不希望生成单例的多个拷贝。
- (id)copyWithZone:(NSZone *)zone
{
    return self;
}
//不被序列化序列化归档
- (id)initWithCoder:(NSCoder*)decoder    
  {    
    
}
// 什么也不做——该单例并不需要一个引用计数(retain counter)
- (id)retain {
    return self;
}
// 替换掉引用计数——这样就永远都不会 release 这个单例。
- (NSUInteger)retainCount {
    return NSUIntegerMax;
}
// 该方法是空的——不希望用户 release 掉这个对象。
- (oneway void)release {
}
//除了返回单例外,什么也不做。 - (id)autorelease {
    return self;
}
@end
```
```
/*********
 宏作用:单例生成宏
 使用方法:http://blog.csdn.net/totogo2010/article/details/8373642
 **********/
#define DEFINE_SINGLETON_FOR_HEADER(className) \
\
+ (className *)shared##className;

#define DEFINE_SINGLETON_FOR_CLASS(className) \
\
+ (className *)shared##className { \
static className *shared##className = nil; \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
shared##className = [[self alloc] init]; \
}); \
return shared##className; \
}
```
.h
```
#import "testSingleton.h"  
  
@implementation testSingleton  
DEFINE_SINGLETON_FOR_CLASS(testSingleton)  
@end   
```
.m
```
#import <Foundation/Foundation.h>  
@interface testSingleton : NSObject  
DEFINE_SINGLETON_FOR_HEADER(testSingleton);  
@end 
```
参考链接


[php常用设计模式](http://www.admin10000.com/document/7115.html)
[IOS单例宏](http://blog.csdn.net/totogo2010/article/details/8373642)
[IOS单例的实现](https://github.com/BeyondVincent/ios_patterns)