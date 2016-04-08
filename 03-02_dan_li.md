# 03-02 单例

什么是单例子？


使用场景
   数据库全局只返回一个实例
   IOS：UserDefualts,UIApplication 等
   Android:sharePerference 等
   
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