# 生成器

####什么是生成器模式？Builder
构建需要多个对象配合的大型对象
####应用场景是什么？
生成一个初始化操作复杂的对象
####PHP写法

#####实例01
```
<?php
 
class Product {
 
    private $name;
 
    public function setName($name) {
        $this->name = $name;
    }
 
    public function getName() {
        return $this->name;
    }
}
 
abstract class Builder {
 
    protected $product;
 
    final public function getProduct() {
        return $this->product;
    }
 
    public function buildProduct() {
        $this->product = new Product();
    }
}
 
class FirstBuilder extends Builder {
 
    public function buildProduct() {
        parent::buildProduct();
        $this->product->setName('The product of the first builder');
    }
}
 
class SecondBuilder extends Builder {
 
    public function buildProduct() {
        parent::buildProduct();
        $this->product->setName('The product of second builder');
    }
}
 
class Factory {
 
    private $builder;
 
    public function __construct(Builder $builder) {
        $this->builder = $builder;
        $this->builder->buildProduct();
    }
 
    public function getProduct() {
        return $this->builder->getProduct();
    }
}
 
$firstDirector = new Factory(new FirstBuilder());
$secondDirector = new Factory(new SecondBuilder());
 
print_r($firstDirector->getProduct()->getName());
// The product of the first builder
print_r($secondDirector->getProduct()->getName());
// The product of second builder
```