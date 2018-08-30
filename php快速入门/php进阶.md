## 数组

### 创建数组

> array();

自动分配ID键

```
$cars=array("Volvo","BMW","Toyota");
```

人工分配ID键

```
$cars = array();
$cars[0]="Volvo";        
$cars[1]="BMW";        
$cars[2]="Toyota";
```

### 获取数组长度

count()函数

```
$cars=array("Volvo","BMW","Toyota"); 
echo count($cars); 
```

## 关联数组

```
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
```

```
$age['Peter']="35";        
$age['Ben']="37";        
$age['Joe']="43";
```

### 遍历打印关联数组

```
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43"); 

foreach($age as $x=>$x_value) 
{ 
echo "Key=" . $x . ", Value=" . $x_value; 
echo "<br>"; 
} 
```

#### foreach数组用法

```
foreach (array_expression as $value)
    statement
foreach (array_expression as $key => $value)
    statement
```

## 数组排序

> - sort() - 对数组进行升序排列
> - rsort() - 对数组进行降序排列
> - asort() - 根据关联数组的值，对数组进行升序排列
> - ksort() - 根据关联数组的键，对数组进行升序排列
> - arsort() - 根据关联数组的值，对数组进行降序排列
> - krsort() - 根据关联数组的键，对数组进行降序排列

### sort()对数组进行升序排序

```
$cars=array("Volvo","BMW","Toyota");
sort($cars);

$numbers=array(4,6,2,22,11);
sort($numbers);

$clength=count($cars);
for($x=0;$x<$clength;$x++)
{
  echo $cars[$x];
  echo "<br />";
}
```

BMW 

Toyota 

Volvo

### rsort()对数组进行降序排序

```
$cars=array("Volvo","BMW","Toyota");
rsort($cars);
$numbers=array(4,6,2,22,11);
rsort($numbers);
```

