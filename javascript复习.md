[TOC]

## 在html中使用javascript

```javascript
<script type="text/javascript">
console.log("hello world");
</script>
```
如果是外链的文件的引用的话:

```javascript
<script src = "http://d3js.org/d3.v3.min.js"
charset= "utf-8"></script>
```

## 语法

1. 区分大小写

2. 注释 (和C语言是一样的)

   ```
   // var name = "zhangsan ";
   ```

3. 花括号 (函数格式建议写成这样)

   ```javascript
   function getName(person){
       return person.name;	
   }		
   ```

4. 变量

```javascript
var a;
var b,c,d;
```

javascript是弱类型的,所有的变量定义都是是使用var既可以是数字也可以是布尔型,还可以是字符串.

5. javascript 有五种基本数据类型:undefined,null,boolean,number,string.还用一种复杂的数据类型:object,变量属于哪一种都可以用typeof来查看

   ```javascript
   var a = 26;
   console.log(typeof a); // number
   ```

   - undefined 

     未初始化的变量面膜人都是undefined,表示未定义.

   - null

     表示一个空对象,如果某个变量将来要为其复制为object类型,将其初始化为null较好.

     ```javascript
     var 0 = null;
     o = {name:"zhangsan",age:19};
     ```

     null的变量属于object类型

   - boolean

   - number 整数和浮点数

   - string 单引号或者是双引号括起来的,字符串长度用length求得

     ```javascript
     str_1.length
     ```

     拼接用(+)来实现

     ```javacript
     console.log(str_1+str_2);//连接起来
     ```

   - object

     对象拥有属性和方法,通过new来创建对象之后,运用赋值来添加属性和方法

     ```javascript
     var person = new Object();
     person.name = "wangwu";
     person.age = 20;
     person.grouUp = function(){
         this.age +=1; //年龄 增加一岁
     }
     ```

6. 操作符

   - 算术操作符
   - 赋值操作符
   - 布尔操作符
   - 关系操作符
   - 条件操作符

7. 语句

   - if-else语句,与C语言一样

   - while 和do-while语句 简单

   - for 和for-in语句

     for-in 主要用于枚举对象的属性和方法

     ```javascript
     for(var prop in person){
         console.log(prop); //输出name,age,growup
     }
     ```

   - switch语句

   - break,continue和label语句

8. 函数

   不需要指定参数的数据类型

9. 对象

10. 数组

## DOM

DOM,指文档对象模型,是针对结构化文档的一个借口,它允许程序和脚本动态地访问和修改文档.

1. 访问和修改HTML元素

   ```javascript
   document.getElementById('myid'); //反返回id为myid的元素
   document.getElementsByTagName("p"); // 返回所有标签为p的元素
   document.getElementsByClassName("myclass"); //返回类为myclass的元素
   ```
   ![image.png](https://upload-images.jianshu.io/upload_images/6836439-137a236883024c59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 添加和删除节点

   使用appendChild()方法添加节点,用removeChild来删除节点

   ```
   var para = document.createElement("p");
   para.innerHTML = "Hello"; //给p的内容赋值
   body.appendChild(para); //添加节点

   body.removeChild(para) // 删除节点
   ```

3. 事件

   ```javascript
   var para = document.getElementById("mypara");
   para.onclick = function(){
       this.innerHTML = "Thank you ";
   }
   ```

   ​

   ![image.png](https://upload-images.jianshu.io/upload_images/6836439-6a2530148d441f6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/6836439-07b7a54df7e46ba8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

