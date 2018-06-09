[TOC]

### 概述

deque 是“double-ended queue”的缩写,和 vector 一样,deque 也支持随机存取。
vector 是单向开口的连续性空间,deque 则是一种双向开口的连续性空间,所谓双向开口,意思是可以在头尾两端分别做元素的插入和删除操作,vector 当然也可以在头尾两端进行插入和删除操作,但是头部插入和删除操作效率奇差,无法被接受。

> deque 和 vector 的最大差异?
> 一在于 deque 允许常数时间内对头端进行元素插入和删除操作。
> 二在于 deque 没有容量的概念,因为它是动态的以分段的连续空间组合而成,随时可以增加一段新的空间并链接起来,换句话说,像 vector 那样“因旧空间不足而重新分配一块更大的空间,然后再复制元素,释放空间”这样的操作不会发生在 deque 身上,也因此deque 没有必要提供所谓的空间保留功能。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-248002f62bf65902.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/6836439-5c28881860739efe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 这个内部的实现是比较复杂的，简单了解即可。

特性

特性总结:

- 双端插入和删除元素效率较高.
- 指定位置插入也会导致数据元素移动,降低效率.
- 可随机存取,效率高.

### 常用API

