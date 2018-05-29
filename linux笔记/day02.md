[TOC]

## 第一部分vim

1. ​

## 第二部分gcc

1. gcc工作流程

   - 预处理: --E (cpp预处理器)
     - 宏替换
     - 头文件展开
     - 注释去掉
     - xxx.c-> xxx.i
       - c文件
   - 编译 --S(gcc编译器)
     - xxx.i -> xxx.s
     - 汇编文件
   - 汇编 -c(as汇编器)
     - xxx.s -> xxx.o
     - 二进制文件
   - 链接(lib连接器)
     - xxx.o -> xxx(可执行)
   - 注意最浪费时间的是编译阶段

2. gcc常用参数

   - -v/--version

   - **-I: 编译的时候指定头文件路径**

   - -c 生成二进制文件 

   - **-o:指定生成文件的名字**

   - **-g:**

     **gdb调试的时候,需要加上**

   - **-D**

     **在编译的时候指定一个宏**

   - -Wall

     - 添加警告信息

   - -On:

     - 优化代码,n是优化级别:1,2,3(最高是3)

     - ```c++
       int a=10;
       int b =a;
       int c =b;
       printf("%d",c);
       //优化:
       printf("%d",10);
       ```

     - ​

## 第三部分- 静态库和动态库的制作和使用

1. 库是什么

   - 二进制文件
   - 将源代码->二进制格式的源代码
     - .c .cpp
   - 加密

2. 如何给用户使用?

   - 头文件
   - 制作出的库

3. 静态库的制作和使用

   - 命名规则:libtest.a
     - lib
     - xxx -库的名字
     - .a
   - 制作步骤:
     - 原材料:源代码 .c .cpp
     - 将.c文件生成.o
       - gcc a.c b.c -c
     - 将.o打包
       - ar -archieve  rcs 静态库名字 原材料
       - ar rcs libtest.a a.o b.o
         - ar -archive
   - 静态库的使用:
     - gcc test.c -I ./ -L./lib -lmycalc -o app
       - -L:指定库的路径
       - -l :指定库的名字,去掉lib和.a

4. 动态库的制作和使用

   1. 命名规则

      - libxxx.so

   2. 制作步骤

      - 将源文件生成.o

        gcc a.c b.c -c -fpic(fPIC)

      - 打包

        gcc -shared -o libxxx.so a.o b.o

   3. 库的使用

      - 头文件a.h

      - 动态库 libtest.so

      - 参考函数声明编程测试程序main.c

        gcc main.c -I ./include -L ./lib -l test -o app

   4. 动态库无法加载:()

      - 使用环境变量

        - 临时设置:

          - export LD_LIBRARY_PATH = ./lib

        - 永久设置:

          - 用户级别

            ~/.bashrc

          - 系统级别

            /etc/profile

      - /etc/ld.so.cache 文件列表

        - 找到一个配置文件
          - /etc/ld.so.conf
          - 把动态库的绝对路径添加到文件中
        - 执行一个命令:
          - sudo ldconfig -v

      - 或者直接放进/lib,/usr/lib目录中就行.

      - 知识点拓展:

        - dlopen,dlclose,dlsym

5. 静态库和动态库的优缺点

   1. 静态库

      - 优点
        - 它被打包到应用程序中,加载速度快
        - 补补程序无需提供静态库,移植方便
      - 缺点:
        - 销毁资源,浪费内存(如果程序加载了重复的库,那么内存中就会加载重复的库,所以现在大部分都不用静态库的原因)
        - 更新,部署,发布麻烦

   2. 动态库

      - 特点:
        - APP制作好后,动态库没有打包到APP中
        - 运行./app的时候,动态库不会被加载,当程序调用动态库中的函数的时候,动态库才会被加载.
        - 程序运行之前,会先判断库是否存在
          - 动态加载器ld-linux-86-64.so.2


      - 优点
        - 可实现进程间资源共享
        - 程序升级简单
        - 程序员可以控制何时加载动态库
      - 缺点:
        - 加载速度慢
        - 发布程序需要提供依赖的动态库