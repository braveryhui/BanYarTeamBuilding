# 01 策略模式

###什么是策略模式？

>     1.将一组特定的行为和算法封装成类，已适应某些特定上下文环境
    2.把算法封装成一个对象，能消除根据数据类型决定使用什么算法，从而避免使用if－else switch－case语句


###策略模式的使用场景？




> 1.控制器和视图之间是策略模式的关系，同样的数据可以展现出来不同的视图 IOS中数据源－TableviewCellAndroid中的Adapter－listItem
 

> 2.需要根据类型封装各种复杂的算法和流程




> 3.多个条件语句if－else或switch 中每个条件判断都需要复杂的流程的时候
  
###PHP实现
####IUserStrategy.php
```
<?php
namespace Banyar\Strategy;
interface IUserStrategy
{
    function showAD();
    function showCategory();
}
```
####MaleStrategy.php
```
<?php
namespace Banyar\Strategy;
class MaleStrategy implements IUserStrategy
{
    function showAD()
    {
      echo "Male showAD <br />";
    }
     function  showCategory()
    {
      echo "Male showCategory<br />";
    }
}

```
####FemaleStrategy
```
<?php
namespace Banyar\Strategy;

class FemaleStrategy implements IUserStrategy
{
  public function showAD()
  {
      echo 'Female AD ..<br />';
  }
  public function showCategory()
  {
    echo 'Female CateGory..<br />';
  }
}
```

###浏览器访问根据参数调用
```
http://localhost/index.php?sex=female

class IndexPage
{
  public $strategy;
  function index()
  {
    $this->strategy->showAD();
    $this->strategy->showCategory();
  }
  function setStrategy(\Banyar\Strategy\IUserStrategy $strategy)
  {
     $this->strategy = $strategy;
  }
}
$strategyString ="Banyar\\Strategy\\".ucfirst($_GET['sex'].'Strategy');
$strategyObj = new $strategyString();
$page = new IndexPage();
$page->setStrategy($strategyObj);
$page->index();
```
####输出 
```
Female AD ..
Female CateGory..
```

###面向对象哪些设计原则

> 解耦、实现控制翻转、依赖倒置






参考
[Rango大话设计模式](http://www.imooc.com/video/4906)
《Object-c编程之道》
[极客学院IOS设计模式](http://www.jikexueyuan.com/course/1864.html)
