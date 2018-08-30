## 基本结构

```c#
using System;
namespace MyApp // 命名空间,其实我没有定义这个,这个程序也是可以运行的
{
    class Program
    {
        static void Main(string [] args) // main方法,程序的入口
        {
        	Console.Write("hello World!");//命令,以分号结束.
        }
    }
}
```

> 引入用的是using 来导入库.还有注意了Main方法是大写的M.

## 关键字

在vs中,关键字都被标识为蓝色的.

## 注释

注释分为：单行注释、多行注释、文档注释。

**单行注释**的符号是2条斜线（请注意斜线的方向），2条斜线右侧的内容就是注释，左侧的代码不会受影响。

 **多行注释**以“/*”开始，以“*/”结束，之间的内容就是注释，可以包含多行。

 **文档注释**写在类、方法或属性（以后会学到）的前面，它的符号是3条斜线“///”。

```c#
using System;
using System.Text;

namespace hello
{
    /// <summary>
    /// 添加的文档注释
    /// </summary>
    class Class1
    {
        static void Main(string[] args)
        {
            /*
             * 多行注释
             */
            Console.Write("hello class`"); // 单行注释.
        }
    }
}

```

## 

## 常量

1. 常量声明必须同时复制
2. 不能修改常量的值
3. 声明常量的关键字是const

## 变量

## 数据类型

基本的数据类型有四种

1. char 字符型
2. string 字符串型
3. int 整形
4. double 浮点型



### 自动类型转换

低精度的可以自动转换为高精度的并且进行计算.