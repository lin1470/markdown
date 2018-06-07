

[TOC]

## 顺序容器

![image.png](https://upload-images.jianshu.io/upload_images/6836439-bbf5a55b544d5186.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> string和vector将元素保存在连续的内存空间中。由于元素是连续存储的，由元素的下标来计算其地址是非常快速的。但是在中间位置添加火删除元素非常耗时：在插入火删除位置要移动所有元素。

> list和forward_list两个容器就是添加和删除都很快，但是不支持随机访问。而且开销很大

> deque支持快速随机访问。删除添加代价高。但deque的两端添加或删除元素快。

**现代c++程序应该使用标准库容器，而不是更原始的数据结构，如内置数组**

![image.png](https://upload-images.jianshu.io/upload_images/6836439-2c49727d0f699109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为什么链表的中间插入或者删除快呢，就是因为他是链表，不是连续的空间内存，只需要找到位置，然后新建内存或者删除节点就行了。

