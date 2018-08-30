### 多个目录，多个源文件

本小节对应的源代码所在目录：[Demo3](https://github.com/wzpan/cmake-demo/tree/master/Demo3)。

现在进一步将 MathFunctions.h 和 [MathFunctions.cc](http://mathfunctions.cc/) 文件移动到 math 目录下。

```
./Demo3
    |
    +--- main.cc
    |
    +--- math/
          |
          +--- MathFunctions.cc
          |
          +--- MathFunctions.h
```

对于这种情况，需要分别在项目根目录 Demo3 和 math 目录里各编写一个 CMakeLists.txt 文件。为了方便，我们可以先将 math 目录里的文件编译成静态库再由 main 函数调用。

根目录中的 CMakeLists.txt ：

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo3)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 添加 math 子目录
add_subdirectory(math)

# 指定生成目标 
add_executable(Demo main.cc)

# 添加链接库
target_link_libraries(Demo MathFunctions)
```

该文件添加了下面的内容: 第3行，使用命令 `add_subdirectory` 指明本项目包含一个子目录 math，这样 math 目录下的 CMakeLists.txt 文件和源代码也会被处理 。第6行，使用命令 `target_link_libraries` 指明可执行文件 main 需要连接一个名为 MathFunctions 的链接库 。

子目录中的 CMakeLists.txt：

```
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)

# 生成链接库
add_library (MathFunctions ${DIR_LIB_SRCS})
```

在该文件中使用命令 `add_library` 将 src 目录中的源文件编译为静态链接库。



### 适用于自己的项目

修改为可以快速进行编译,并且可以自动构造了makefile比较,不用编写makefile.

项目树也是类似于上面的步骤:

```cmake
./practise
    |
    +--- chapter1/
   			|
            main.cc(运行练习代码)
    |
    +--- tool/
          |
          +--- ....cc(其他实现文件)
          |
          +--- apue.h
```

#### chapter1

有这么一个cmakeLists.txt:

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo3)
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 添加 math 子目录,注意这个cassdk.out指定的是文件树之外的位置
add_subdirectory(../tool,cassdk.out)
# 指定生成目标 
add_executable(Demo3 main.cc)
# 添加链接库
target_link_libraries(Demo3 apue)

```

这个文件会自动进入../tool文件夹然后执行这个文件的cmakeLists.txt,所以在tool这个文件中也要有CMakeLists.txt

#### tool

这里也有一个CMakeLists.txt:

```cmake
cmake_minimum_required(VERSION 3.0)                           
# 查找当前目录下的所有源文件                                   
# 并保存名称到DIR``````                                     
aux_source_directory(. DIR_LIB_SRCS)             
# 生成链接库                                                 
add_library(apue ${DIR_LIB_SRCS}) 
```



这里这个文件只要一执行就会生成静态链接库libapue.a



### 命令执行

```
# 在chapter中
cmake .
# 然后执行makefile文件就行
make 
```

以后要是进行修改的话,那么运行就只需要修改CMakeLists.txt文件中的可执行文件一行

```cmake
add_executable(Demo3 main.cc)
```

修改文件中的main.c的代码文件即可.