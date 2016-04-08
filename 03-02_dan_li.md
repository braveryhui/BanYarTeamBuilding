# 03-02 单例

什么是单例？
  通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源，也可以有多个实例MutiTon
 

使用场景
   数据库全局只返回一个实例
   IOS：UserDefualts,UIApplication 等
   Android:sharePerference 等
核心点
1.复用 （全局唯一 也可以有多个实例）
2.防止外部调用构造方法
3.防止被clone
4.防止被反序列化
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