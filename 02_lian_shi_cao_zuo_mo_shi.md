# 02 链式操作模式


####什么事链式操作？
   
> 多条件链接组合操作


####使用场景？
  
>  数据库的多条件组合查询
   
###PHP写法
```
class Datebase
{
  function  where($where)
  {
    echo '<br />'.$where.'<br />';
    return $this;
  }
  function order($order)
  {
    echo $order."<br />";
    return $this;
  }
  function limit($limit)
  {
    echo $limit."<br />";
    return $this;
  }
}
```
###调用
```
$db = new Imooc\Datebase();
$db->where('id=123')->order('order by desc')->limit('limit 10');
```
###输出
```
id=123
order by desc
limit 10
```