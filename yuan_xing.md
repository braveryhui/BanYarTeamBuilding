# 原型

####什么是原型模式（ProtoType）？
用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象

####应用场景？
1.当一个类需要同时展现多种状态的时候，最佳方式是去clone一个对象，而不是去new这样节省对象的初始化操作，内存拷贝 提升创建对象的速度
####PHP写法
php使用clone关键字来实现
####实例01
``` php
<?php  
  
//声明一个克隆自身的接口  
interface Prototype {  
    public function copy();   
}     
  
//实现一个克隆自身的操作  
class ConcretePrototype implements Prototype {  
    private $name;  
      
    function __construct($name){  
        $this->name = $name;  
    }  
      
    function getName(){  
        return $this->name;  
    }  
      
    function setName($name){  
        $this->name = $name;  
    }  
      
    //克隆  
    function copy(){  
        return clone $this;  
    }  
}  
  
//调用 
class Client {  
      
    public static function getCloneObj(){  
          
        $pro = new ConcretePrototype('prototype');  
        $pro2 = $pro->copy();  
        echo $pro->getName();  
        echo $pro2->getName();  
    }   
}  
  
Client::getCloneObj();

```
####实例02
``` php
class  Object
{
  static function  init()
  {
    self::loadBall();
    self::loadCavase();
    self::loadAction();
  }
}
$prototype = new Object();
$prototype::init();
$obj01 = clone $prototype;
$obj02 = clone $prototype;
$obj01::test();
$obj02::test();
```