[TOC]

## 第一. makefile

### 1. make

- gcc-编译器
- make-linux自带的构建器
  - 构建的规则在makefile中(项目文件中有makefile)

### 2. makefile文件命名

- makefile
- Makefile

### 3. makefile中的规则

gcc a.c b.c c.c -o app

- 三部分:目标,依赖,命令

  - 目标:依赖

    (tab缩进)命令

    **app:a.c b.c c.c**

    	**gcc a.c b.c c.c -o app**

- makefile中由一条或者多条规则构成

- ​

### 4. makefile编写

- 第一个版本:

  ![image.png](https://upload-images.jianshu.io/upload_images/6836439-f151a3d4fba19b22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 缺点:效率低,修改一个文件,所有文件都会被全部重新编译

- 第二个版本:

  ![image.png](https://upload-images.jianshu.io/upload_images/6836439-58a1d9026c213f92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 工作原理:

  - 检测依赖是否存在:
    - 向下搜索下边的规则,如果有规则用来生成查找的依赖,执行规则中的命令
    - 依赖存在,判断是否需要更新:(直接根据目标和依赖的事前的前后来判断是否需要根性)
      - 原则:目标时间>依赖时间
        - 反之,则更新

- 缺点:

  冗余

- 第三个版本:

  - 自定义变量: 

    obj = a.o b.0 c.0

    obj =10

    变量的取值: aa =$(obj)

  - makefile 自带的变量:大写

    - CPPFLAGS
    - CC

  - 自动变量:

    - $@:规则中的目标

    - $<:规则中的一个依赖

    - $^:规则中的所有依赖

    - ![image.png](https://upload-images.jianshu.io/upload_images/6836439-578abd584c150ec0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    - 匹配模式:

      %.o:%.c

      第一次:main.o 没有

      main.o:main.c

      	gcc -c main.c -o main.o

      第二次:add.o

      add.o:add.c

      	gcc -c add.c -o add.o

    - 可移植性交差

- 第四个版本

  - makefile所有函数都有返回值
  - 查找指定目录ina执行类型的文件
    - src = $(wildcard ./*.c)
  - 匹配替换
    - obj = $(patsubst %c,%.o $(src))
  - ![image.png](https://upload-images.jianshu.io/upload_images/6836439-eab08eed99e0d9be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 缺点:不能清理项目

- 第五个版本

  - 让make生成不是终极目标的目标
    - make 目标名


  - 编写一个清理项目的规则

    clean:

    -rm *.o app -f

  - 声明伪目标

    .PHONY:clean

  - -f:强制执行

  - 命令前加-

    - 忽略前面执行的错误


## 第二GDB调试

### 1. gcc *.c -o app -g

-g :会保留函数名和变量名

### 2.启动gdb 

gdb 可执行程序名字

a. gdb app

b. 给程序传参数: set args xxx xxx

### 3. 查看代码--list

- 当前文件:

  l

  l 行号

  l 函数名

- 非当前文件:

  - l 文件名:行号
  - l 文件名:函数名

- 设置显示函数:

  - set listsize n
  - show listsize

### 4.断点操作 -break /b

- 设置断点:

  - b行号
  - b 函数名
  - b 文件名: 行号
  - b 文件名: 函数名

- 查看断点

  - info(i) b

- 删除断点

  - d num(断点编号)

  - 删除多个:

    d num1 num2

    d num1-num2

  - 设置断点无效:

    - dis num

  - 断点生效:

    - ena num

  - 设置条件 断点

    b 行号if变量 == var


### 5.调试相关命令

- 跑起来

  start 运行一行,停止

  run -r 停在第一个断点的位置


- 打印变量的值

  - p变量名

- 打印变量的类型:

  - ptype变量名

- 向下单步调试:

  - next -n

    不会进入函数体

  - step -s

    会进入到函数体内部

    跳出去用finish

    - 如果出不去,看一下函数体中循环中是否有断点,如果有就删掉,或者设置无效

    ```c
    int a =3;
    int b;
    ```

- 继续运行gdb,停在下一个断点的位置

  continue -c

- 退出gdb

  quit -q

- 变量的自动显示

  - display 变量名
  - 取消:undisplay 编号
    - i display

- 从循环体中直接跳出:

  - until 

    不能有断点

  - 直接设置变量等于某一个值

    - set var 变量名= value


## 三. 文件IO

### 1. 

### 2. 

![image.png](https://upload-images.jianshu.io/upload_images/6836439-3dcd1ba5a9db46f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

MMU内存管理单元(内存的映射机制)->类似于deque

### 3.内核区

PCB进程控制块里面有文件描述符表,每打开一个文件就会在里面占位一个.

![image.png](https://upload-images.jianshu.io/upload_images/6836439-5f502efea1cd2426.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

前三个都是被占用了,后面打开的从3开始.

pcb:结构体

一个进程有一个文件描述符表:1024

- 前三个被占用(stdin,stdout,stderr)
- 文件描述符作用:寻找磁盘文件

#### 1.open/close

open函数是变参函数

- 函数原型:

  - int open(const char *pathname,int flags);
  - int open(const char *pathname,int flags,mode_t mode);

- 参数:

  - flags:

    - 必选项 O_RDONLY,O_WRONLY,O_RDWR

    - 可选项

      - 创建文件:O_CREAT
        - 穿件文件时检测文件是否存在:O_EXCL
        - 如果文件存在,返回-1
        - 必须于O_CREAT一起使用
      - 追加文件:O_APPEND
      - 文件截断:O_TRUNC
      - 设置非阻塞:O_NONBLOCK

    - mode 

      - mode & ~umask

        0777& ~0002

        111111111

        & 111111101

        111111101


#### 2.read函数

- 函数原型:ssize_t read(int fd,void *buf,size_t count);
  - 参数:
    - fd:open返回值
    - buf:缓冲区,存储要读取的数据
    - count:缓冲区能存储的最大字节数sizeof(buf)
  - 返回值:
    - -1:失败
    - 成功:
      - ->0独处的字节数
      - =0:文件读完了

#### 3.write

- 函数原型:ssize_t write(int fd,const void *buf,size_t count);
  - 参数:
    - fd:open返回值
    - buf:要写到文件的数据
    - count:strlen(buf)
  - 返回值:
    - -1:失败
    - ->0:写入到文件的字节数

#### 4.lseek



```

```

