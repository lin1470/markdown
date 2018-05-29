[TOC]

# 一.文件目录

1. 根目录表示方式: /

2. 常见目录

   /bin:binary:二进制文件,可执行程序,shell命令

   /dev: device:常见的设备文件

   /lib:linux运行的时候需要加载的一些动态库

   /mnt:手动的挂载目录,嵌入式需要手动挂载

   /media:外设的自动挂载目录

   /root:linux的超级用户的家目录

   /usr: unix system resource 资源目录

   - 头文件 -stdio.h stdlib.h
   - 游戏 
   - 用户安装的程序 /usr/local

   /etc:存放配置文件

   - /etc/passwd
   - /etc/group
   - man 5 passwd

   /opt: 安装第三方应用程序

   /home: linux所有用户的家目录

   - 用户家目录(宿主目录)
     - /home/bruce

   /tmp: 存放临时文件

   /boot:启动目录

3. 用户当前工作目录

   1. 相对路径

   2. 绝对路径

   3. .和..

      - . 代表当前目录
      - .. 当前目录的上一级目录


      - ![Selection_128](/home/bruce/Pictures/Selection_128.bmp)

   4. 用户的家目录:

      - Bruce:当前登录用户
      - @:在
      - Ubuntu 就是系统名字
      - ~ 用户家目 

   5.  

4. 常用目录相关命令:

   1. tree 

      - 查看目录内容
      - tree 查看当前目录
      - tree dir 查看指定目录

   2. ls 目录或者文件名

      - 功能:查看文件或者目录

      - 语法:

      - 参数:

        - -a显示所有文件

          - 隐藏文件 带. 

        - -l 详细信息

          ![Selection_129](/home/bruce/Pictures/Selection_129.bmp)

          - 第一个字符:文件类型
            - 七种类型:
            - 普通文件:-
            - 目录:d
            - 符号链接:-l
            -  管道:p
            - 套接字:s
            - 设备:b
          - rw-:文件所有这权限
          - rw-:文件所属组权限
          - r--:其他组权限
          - 1:硬链接计数
          - bruce:文件所有者
          - bruce:文件所属组的名字
          - 1714:文件大小(如果是文件目录都是4k,本身的大小不包括里面的内容)
          - 日期
          - 文件名

   3. cd 

      1. cd目录
      2. 如何进入家目录
         - cd绝对路径
         - cd ~
         - cd
      3. 如何切换两个临近的目录
         - cd -

   4. pwd 输出当前路径 print working directory

   5. mkdir 创建目录

      - mkdir -p aa/bb/cc 创建多级目录

   6. touch 文件

      - touch 文件名,如果文件不存在,则创建
      - 如果存在了:更新文件的时间

   7. rmdir 文件目录

      - 只能创建一个空目录

   8. rm 

      - -r 递归删除目录
      - 文件名 -i 添加信息
      - 注意:
        - 删除之后,很难恢复
        - 所以要用参数 -i

   9. cp -- 拷贝 

      - cp file1 file(不存在)新创建

      - cp file1 file(存在) 覆盖原来文件

      - cp file1 dir(存在的目录)

        拷贝file1到dir的目录中去

      - cp dir1 dir2 -r(拷贝整个目录到指定路径中去)

      - cp dir1 dir2 -r(dir2不存在的话)创建dir2,将dir1的内容拷贝到dir2中,不包括dir1.

   10. mv 

       - mv file file1(不存在)改名
       - mv dir dir1(不存在) 改名
       - mv file dir1(存在)移动
       - mv dir dir2(存在)移动
       - mv file file1(存在)覆盖了file1

   11. cat 把文件内容打印到到终端

       - 适用于文件内容比较少的情况

   12. more 文件名

       - 回车向下浏览一行
       - 空格:翻页向下

   13. less 文件名

       - 向下滚动一行:回车,ctrl+n
       - 向上滚动一行:ctrl+p
       - 向下翻页:空格或者pagedown
       - 向上翻页:pageup
       - 退出:q

   14. head默认显示开始的10行

       head -5 显示5行

   15. tail 默认10行

   16. ln -s 文件名 快捷方式名字 (软链接,相对路径)l

       - 存的是路径,大小就是路径名字大小(ln -s s a.soft)
       - ln -s ~/aa a.soft 使用的是绝对路径
       - 目录也是可以创建软链接的

   17. 硬链接

       - ln 文件名(无所谓绝对和相对) 链接名字
       - 创建了一个文件的别名,指向同一个inode节点
       - 1. 创建一个文件,硬链接计数:1
         2. 创建一个硬链接:+1
         3. 删除一个硬链接:-1
         4. 再删除硬链接计数对应的文件:0
       - 使用场景:
         - 磁盘上有一个文件 /home/bruce/hello
         - 在其他多个目录中进行管理hello,并且能实时同步(牛逼哈哈)
         - ​