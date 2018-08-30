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
5. bool 布尔类型



### 自动类型转换

低精度的可以自动转换为高精度的并且进行计算.例如

```
double d =2.5;
double x = d+1;
```

#### 但是高精度的无法自动转为低精度的

```
int x = 3.5 + 1;
```

这时候需要显示的转换

```
int x = (int)3.5+2;
```

## 标志符的命名规则

与C语言是一样的.



## 变量

### 简单类型

整形

1. sbyte
2. byte
3. short
4. ushort
5. int
6. uint
7. long
8. ulong

浮点型

1. float
2. double
3. decimal

还有三种简单类型

1. char
2. bool
3. string

> 其中string的字符数量是没有上限的,因为它可以使用可变大小的内存.

基本读入和输出语句

```c#
static void Main(string[] args)
        {
            double firstNumber, secondNumber;
            string userName;
            Console.WriteLine("Enter your name:");
            userName = Console.ReadLine();
            Console.WriteLine($"Welcome {userName}!");
            Console.WriteLine("Now give me a number:");
            firstNumber = Convert.ToDouble(Console.ReadLine());
            Console.WriteLine("Now give me another number:");
            secondNumber = Convert.ToDouble(Console.ReadLine());
            Console.WriteLine($"The sum of {firstNumber} and {secondNumber} is {firstNumber + secondNumber}.");
            Console.WriteLine($"The result of subtracting {secondNumber} from {firstNumber} is {firstNumber - secondNumber}.");
            Console.WriteLine($"The product of {firstNumber} and {secondNumber} is {firstNumber * secondNumber}.");
            Console.WriteLine($"The result of dividing {firstNumber} by {secondNumber} is {firstNumber / secondNumber}.");
            Console.WriteLine($"The remainder after dividing {firstNumber} by {secondNumber} is {firstNumber % secondNumber}.");
            Console.ReadKey();
        }
```

> 注意这个,输入时从字符串转到Int或者是double,然后输出的话,采用的是$符号的形式,这样可以大大减少输出的格式长度,减少代码量.
>
> $符号的表达式后面的{}里面是可以接一个个的表达式的.



## 控制流程

### switch语句

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using static System.Console;

namespace Ch04Ex03
{
    class Program
    {
        static void Main(string[] args)
        {
         const string myName = "benjamin";
         const string niceName = "andrea";
         const string sillyName = "ploppy";
         string name;
         WriteLine("What is your name?");
         name = ReadLine();
         switch (name.ToLower())
         {
            case myName:
               WriteLine("You have the same name as me!");
               break;
            case niceName:
               WriteLine("My, what a nice name you have!");
               break;
            case sillyName:
               WriteLine("That's a very silly name.");
               break;
         }
         WriteLine($"Hello {name}!");
         ReadKey();
        }
    }
}

```

> 明显这里可以看出就是,这个东西是没有case的类型限制的.不详C语言这个是有数字整形的限制的.

## 类型转换

### 隐式转换

精度交低的或者是低存储的可以隐式转为高精度的或者是高存储的,因为没有歧义

比如

| 类型  | 安全转为                                         |
| ----- | ------------------------------------------------ |
| byte  | short,ushort,init,uint,long,float,double,decimal |
| sbyte | short,int,long,float,double,decimal              |
| char  | ushort,int,uint,long,ulong,float,double,decimal  |

```
       byte destinationVar = 9;
            short sourceVar = 7;
            sourceVar = destinationVar; //隐式转换不会报错
            destinationVar = sourceVar; // 显示转换.
```



### 显示转换

```
byte destinationVar;
short sourceVar = 7;
destinationVar = sourceVar;
```

### check

```
byte destinationVar = 9;
            short sourceVar = 281;
            //sourceVar = destinationVar; //隐式转换不会报错
            destinationVar = checked((byte)sourceVar); // 显示转换.
            Console.WriteLine($"sourceVar:{sourceVar}");
            Console.WriteLine($"destinationVar:{destinationVar}");
```

使用check是可以进行类型转换是否出错检查,如果检查不通过的话,那么会上报出异常来的.

如果使用uncheck的话,就是和默认没有检查是一样的.

```c#

short shortResult, shortVal = 4;
            int integerVal = 67;
            long longResult;
            float floatVal = 10.5F;
            double doubleResult, doubleVal = 99.999;
            string stringResult, stringVal = "17";
            bool boolVal = true;
            doubleResult = floatVal * shortVal;
            Console.WriteLine($"implicit,->double:{floatVal}*{shortVal}->{doubleResult}");
            shortResult = (short)floatVal;
            Console.WriteLine($"Explicit, -> short: {floatVal} -> {shortResult}");

            stringResult = Convert.ToString(boolVal) + Convert.ToString(doubleVal);
            Console.WriteLine($"Explicit, ->string:{boolVal}+{doubleVal} ->" +
                $"+{stringResult}");
            longResult = integerVal + Convert.ToInt64(stringVal);
            Console.WriteLine($"mixed,->long:{integerVal}+{stringVal}->{longResult}");

```

