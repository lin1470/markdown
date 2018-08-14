[TOC]

## open和openat函数

```c
int open(const char path,int oflag..mode)

int openat(int fd,const charpath,int oflag...mode)

```

oflag的指定的打开文件的权限,一般常用的是:

- O_rdonly
- O_WRONLY
- O_RDWR
- O_EXEC
- O_SEARCH(用于搜索)

mode参数指定的是创建文件的文件权限,用户所属问题

## create函数

int create(const char*path,mode_t mode)失败返回-1



## close函数

int close(int fd) 失败返回-1,成功0

## lseek函数

off_t lseek(int fd,offt_t offset,int whence)

设置文件描述符的偏移量

## read函数

ssize_t read(int fd,void*buf,size_t nbytes)

buf的大小会影响效率

## write函数

ssize_t write(int fd,const void*buf,size_t nbytes)

指定写入字符

## 文件共享

> 1. 每一个进程拥有一个文件描述符表,每一项指向一个文件表项,表项中有v节点指针指向v节点,v节点指向INode
> 2. 所以不同进程可以创建不同的文件表项,实际上指向的同一个inode

## 原子操作

原子操作防止两个函数调用之间被cpu中断,切换到另个一个线程,导致数据混乱

## dup和dup2

```c
int dup(int fd);
int dup2(int fd1,int fd2);
```

1. 返回最小可用
2. 先关闭fd2,然后fd2也指向同一个个文件表项

## fcntl函数

```c
int fcntl(int fd,int cmd...int arg);
```

小复杂

主要看参数cmd

> 1. cmd = f_dupfd/f_dupfd_cloexec
> 2. cmd = f_getfd/f_setfd
> 3. cmd=f_getfl/f_setfl
> 4. cmd=f_getown/f_setwon
> 5. cmd=f_getlk,f_setlk,f_setlkw

