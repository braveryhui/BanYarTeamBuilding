# 03 接口&抽象类&类

###什么是接口？

>     1.接口中所有的方法都是抽象方法，用Interface来声明，
    2.接口是用来建立类与类之间的协议，它所提供的只是一种形式，而没有具体的实现。
    3.同时实现该接口的实现类必须要实现该接口的所有方法，通过使用implements关键字，他表示该类在遵循某个或某组特定的接口，接口可以继承接口，实现接口扩展，用extends来实现接口的扩展。
    4.这种抽象类中只包含抽象方法和静态常量。
    
###使用场景？

> 1. 多数据库驱动的时候，接口方法
2. IOS中Tableview的数据源接口
3. JAVA中的 Runable接口 和 listView的Adapter等等



###如何正确使用？

> 1. Interface的方所有法访问权限自动被声明为public。确切的说只能为public**实现接口的类中接口都得方法也必须是public**，当然你可以显示的声明为protected、private，但是编译会出错,任何实现接口的类都要实现接口中所定义的所有方法，否则该类必须声明为 abstract 
2. 接口中可以定义“成员变量”，或者说是不可变的常量，因为接口中的“成员变量”会自动变为为public static final。可以通过类命名直接访问：ImplementClass.name。
3. 接口中不存在实现的方法。
4. 实现接口的非抽象类必须要实现该接口的所有方法。抽象类可以不用实现。
5. 不能使用new操作符实例化一个接口，但可以声明一个接口变量，该变量必须引用（refer to)一个实现该接口的类的对象。可以使用 instanceof 检查一个对象是否实现了某个特定的接口。例如：if(anObject instanceof Comparable){}。PHP中同样用instanceof IOS 使用isKindOf
6. 在实现多接口的时候一定要避免方法名的重复。




###PHP实现
```
<?php
interface IDataBase
{
  const IPadress  = @"192.168.1.1"; //静态常量
  function connect(){};
  function query(){};
  static function getLinkID(){};
  function close(){};
}

/**
 * 扩展接口 IDataBase
 */
interface IEDataBase extends IDataBase
{
  function paraSql(){};
}

/*
*  实现接口
**/
class Mysqli implements IEDataBase
{
  function connect(){
    echo 'Mysqli connent';
  };
  function query(){
    echo 'Mysqli query';
  };
  static function getLinkID(){
     echo 'Mysqli getLinkID';
  };
  function close(){
     echo 'Mysqli close';
  };
  function paraSql(){
     echo 'Mysqli paraSql';
  };
}

/*
*  实现接口
**/
class PDO implements IEDataBase
{
  function connect(){
    echo 'PDO connent';
  };
  function query(){
    echo 'PDO query';
  };
  static function getLinkID(){
     echo 'PDO getLinkID';
  };
  function close(){
     echo 'PDO close';
  };
  function paraSql(){
     echo 'PDO paraSql';
  };
}

/**
 * 对外统一 操作的数据库方法
 * 比如TP 里面的 大U方法获取 用户数据我
 */
class DB
{
    static $db = null;
    public $sql = null;
    function  __construct($config)
    {
      switch ($config['DBConnectType']) {
        case 'mysqli':
            self::$db = new Mysqli();
          break;
        case 'pdo':
            self::$db = new PDO();
          break;
        default:
          break;
      }
      self::$db->connect();
    }

    fucntion where($where){
      echo 'do Where<br />';
      $this->sql .= 'do Where';
      return $this;
    }
    funtion order($order){
      echo 'do Order <br />';
      $this->sql.= 'do Order';
      return $this;
    }
    function  querySql($sql)
    {
      self::$db->query($this->sql);
    }
    function __destruct()
    {
        self::$db->close();
    }
}

//调用
$db  = new  DB();
$db->where('do where')->order('order desc')->querySql();

```
###是抽象类?

> 1. 抽象类往往用来表征对问题领域进行分析、设计中得出的抽象概念，是对一系列看上去不同，但是本质上相同的具体概念的抽象。
2. 抽象类体现了数据抽象的思想，是实现多态的一种机制。它定义了一组抽象的方法，至于这组抽象方法的具体表现形式有派生类来实现。



###使用场景？

>  1. 实现子类的多态 astract方法定义
 2. 抽象子类共有(都需要用)的方法，如TP的DB类抽象出来了各种para*的方法 


###如何正确的使用？

> 1. 在面向对象领域由于抽象的概念在问题领域没有对应的具体概念，所以用以表征抽象概念的抽象类是不能实例化的。
2. 抽象类不能被实例化，实例化的工作应该交由它的子类来完成，它只需要有一个引用即可。
3. 抽象方法必须由子类来进行重写。
4. 只要包含一个抽象方法的抽象类，该方法必须要定义成抽象类，不管是否还包含有其他方法。
5. 抽象类中可以包含具体的方法，当然也可以不包含抽象方法。
6. 子类中的抽象方法不能与父类的抽象方法同名。
7. abstract不能与final并列修饰同一个类。
8. abstract 不能与private、static、final或native并列修饰同一个方法




###写法
    1

#接口 & 抽象类 & 实体类的关系



> 1. 抽象类相对于接⼝口的好处：抽象类包含了⼀一些⽅方法的实现，即：⾮非抽象⽅方法，⽽而接⼝口中的所有的⽅方法都是抽象的，所以抽象类可以⼦子类中共同的逻辑向上提级，在抽象类中实现了代码的复⽤用
2. 抽象类和接⼝口相对于实体类的好处：通过⽅方法覆盖类实现多态，也就是运⾏行时绑定
3. 抽象层次不同。接⼝口是定义⾏行为的，抽象类是实现⾏行为的，实体类是执⾏行⾏行为的，抽象类是对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。
4. 跨域不同。抽象类所跨域的是具有相似特点的类，而接口却可以跨域不同的类。我们知道抽象类是从子类中发现公共部分，然后泛化成抽象类，子类继承该父类即可，但是接口不同。实现它的子类可以不存在任何关系，共同之处。例如猫、狗可以抽象成一个动物类抽象类，具备叫的方法。鸟、飞机可以实现飞Fly接口，具备飞的行为，这里我们总不能将鸟、飞机共用一个父类吧！所以说抽象类所体现的是一种继承关系，要想使得继承关系合理，父类和派生类之间必须存在"is-a" 关系，即父类和派生类在概念本质上应该是相同的。对于接口则不然，并不要求接口的实现者和接口定义在概念本质上是一致的， 仅仅是实现了接口定义的契约而已。
5.  设计层次不同。对于抽象类而言，它是自下而上来设计的，我们要先知道子类才能抽象出父类，而接口则不同，它根本就不需要知道子类的存在，只需要定义一个规则即可，至于什么子类、什么时候怎么实现它一概不知。比如我们只有一个猫类在这里，如果你这是就抽象成一个动物类，是不是设计有点儿过度？我们起码要有两个动物类，猫、狗在这里，我们在抽象他们的共同点形成动物抽象类吧！所以说抽象类往往都是通过重构而来的！但是接口就不同，比如说飞，我们根本就不知道会有什么东西来实现这个飞接口，怎么实现也不得而知，我们要做的就是事前定义好飞的行为接口。所以说抽象类是自底向上抽象而来的，接口是自顶向下设计出来的

参考链接
[java](http://blog.csdn.net/chenssy/article/details/12858267)
[PHP](http://blog.csdn.net/klinghr/article/details/5212976)
《细说PHP》
Tinkphp框架实现

