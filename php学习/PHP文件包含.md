# **文件包含**

> **思考**：PHP的函数要求是函数必须在内存中才能调用，但是函数是一个一个写在对应的PHP文件中的，那其他文件中的代码必须要复制代码过来才能访问吗？

> **引入**：如果一个文件中对应的功能已经写好了，那么想要在其他PHP文件中使用，必须要保证该文件中的代码都进入到内存，而且彼此间有关联才可。这个使用PHP提供了一种解决方案，就是文件包含。

## **文件包含【掌握】**
> **定义**：文件包含，就是在一个要运行的PHP脚本中，去将另外一个PHP脚本中的代码拿过来，并且可以使用其被包含的文件里的内容，或者说将自己的内容能够在另外一个被包含的文件中使用。

1.文件包含基本语法：PHP中提供了四种文件包含的方法，分别是include和include_once，require和require_once，其中四种方式的用法完全一样    
include/include_once/require/equire_once '文件所在路径及文件名';       
include/include_once/require/equire_once('文件所在路径及文件名');    	
```PHP
<?php
//文件包含
include 'index.php';    //包含index.php文件（当前PHP文件同级目录）
?>
```
2.文件包含的意义：文件包含的目的有两个
* 向上包含：即先包含某个文件，目的为了使用某个文件中的代码或者数据（使用公共代码）

```PHP
index.php
<?php
//定义函数
function show(){
    echo 'hello world';
}
?>

include.php
<?php
//包含index.php文件
include_once 'index.php';
//调用index.php中的函数
show();                    //输出hello world
?>
```
* 向下包含：即先写好代码，后包含文件，目的是为了在被包含文件中使用当前的数据（使用已产生数据）

```PHP
index.php
<?php
//获取数据
$info = 'hello world';
//包含文件
include_once 'include.php';
?>

include.php
<html>
<head></head>
<body>
<?php
    echo $info;             //输出hello world
?>
</body>
</html>
```
3.文件包含的语法区别：四种包含方式都能够包含文件并使用
* include和require的区别在于，如果包含的文件不存在的时候，include只是报警告错误，而不影响自身代码执行；而require会报致命错误，而且中断代码执行
* include和include_once区别：include不论如何都会执行包含操作，而include_once会记录是否已经包含过对应文件，对同一文件多次包含只操作一次（对于函数/类这种结构不允许重复的，是个好方法）。

4.文件包含原理：文件包含本质就是将被包含文件的所有代码，在进行包含操作那一行全部引入并运行。但是文件包含语句是在运行时才会执行，因此不能先访问被包含文件中的内容，后包含文件。
```PHP
index.php
<?php
function show(){
    echo __FUNCTION__;
}
?>

include.php
<?php
show();                    //错误：系统找不到函数
//引入
include_once 'index.php';

show();                    //show：正确，先引入后使用
?>
```    
5.练习：在a.php文件中写好九九乘法表函数，在b.php中包含该文件，然后在c文件中显示九九乘法表。即b包含a也包含c

> **总结**：文件包含是一种真正意义上方便我们进行模块化开发（代码功能分开到不同的文件中使用函数进行管理维护），同时又可以实现代码复用的功能。能够帮助开发者节省大量时间和工作量。

****

> **思考**：文件包含的时候是如何找到被包含的文件的呢？难道所有的开发文件都要放到一起吗？

> **引入**：实际开发中，非常小型的项目有可能所有文件放到一起，但是大中型项目动辄成百上千个文件，不可能放到一起的。而且还要面临用户本地开发与实际部署到服务器的路径不同的问题。所以在文件包含的时候一定要注意路径问题。

## **路径问题【掌握】**
> **定义**：路径问题，是指PHP在文件包含的时候，采用什么样的方式去寻找文件的问题。在系统中，路径通常分为两种：绝对路径和相对路径。

1.绝对路径：绝对路径包含两种，一种是本地绝对路径，还有一种是互联网绝对路径
* 本地绝对路径：从盘符根目录到文件的路径。Windows下是逻辑盘符如D:/server/Web/index.php；而Linux下是根目录如/home/Web/index.php
* 互联网绝对路径：URL，如www.taobao.com/index.php

```PHP
D:/server/Web/index.php
<?php
function show(){
    echo __FUNCTION__;
}
?>

D:/server/Web/include.php
<?php
include_once 'D:/server/Web/index.php';    //绝对路径包含
?>
```
2.相对路径：即被包含文件相对于当前文件所在的路径，通常有三种：
* ./：表示同级目录（当前文件所属文件夹），每个文件夹下都有“.”文件，代表当前目录
* ../：表示上级目录（当前文件所属文件夹的上级文件夹），每个文件夹下都有“..”文件，表示上级目录
* 什么都没有：就是表示同级目录。区别./在于./会自动匹配任意目录下的.文件夹，而什么都没有则只会从自身文件所在目录开始（比./安全）

```PHP
D:/server/Web/index.php
<?php
function show(){
    echo __FUNCTION__;
}
?>

D:/server/Web/include.php
<?php
include_once 'index.php';                //相对路径包含
include_once './index.php';              //相对路径包含
?>
```
3.绝对路径和相对路径的区别
* 相对路径效率高：相对路径只要按照当前文件位置偏移寻找即可
* 相对路径不安全：相对路径一旦嵌套，./这个当前目录就会发生改变
* 绝对路径安全：不会因为嵌套而出现路径变化
* 绝对路径效率低：因为一定会要从根目录开始逐层寻找

4.相对路径的嵌套包含问题：相对路径./和../会因为文件被包含之后发生改变（被不同文件夹的包含，同一文件夹下不会发生改变）
```PHP
D:/server/Web/parent/son/son.php
<?php
echo __DIR__,'son';
?>

D:/server/Web/parent/parent.php
<?php
echo __DIR__,'parent';
include_once './son/son.php';        //包含son.php
//此时，单独访问parent.php没有问题
?>

D:/server/Web/index.php
<?php
include_once './parent/parent.php';  
//此时，访问index.php出错：parent.php被index.php包含后，其相对路径发生改变。./变成index.php所在路径D:/server/Web，再次包含./son/son.php路径变成了D:/server/Web/son/son.php，而这个路径不存在

//解决方案：1.不使用./；2.使用绝对路径
?>
```
5.练习：使用绝对路径和相对路径进行多层级嵌套包含

> **总结**：在进行文件包含时，不建议使用./和../进行文件包含。因为在实际开发中嵌套包含文件在所难免，而绝对路径包含可以完美避免这一问题。另外绝对路径建议使用__DIR__魔术常量，可以保证代码迁移不会被操作系统和磁盘所限制。




