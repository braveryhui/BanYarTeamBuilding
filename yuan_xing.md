# 原型

####什么是原型模式（ProtoType）？
用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象

####应用场景？

####PHP写法

```
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
  
//客户端  
class Client {  
      
    public static function getOBJ(){  
          
        $pro = new ConcretePrototype('prototype');  
        $pro2 = $pro->copy();  
        echo $pro->getName();  
        echo $pro2->getName();  
    }   
}  
  
Client::main();

```