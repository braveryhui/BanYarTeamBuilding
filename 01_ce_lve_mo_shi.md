# 01 策略模式

###什么是策略模式？
    1.将一组特定的行为和算法封装成类，已适应某些特定上下文环境
    2.把算法封装成一个对象，能消除根据数据类型决定使用什么算法，从而避免使用if－else switch－case语句
###策略模式的使用场景？
    
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
####
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




