[TOC]



## 栈

### 概述

stack 是一种先进后出(first in last out,FILO)的数据结构,它只有一个出口,stack 只
允许在栈顶新增元素,移除元素,获得顶端元素,但是除了顶端之外,其他地方不允许存取元素,只有栈顶元素可以被外界使用,也就是说 stack 不具有遍历行为,没有迭代器。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-28d9c459ab60a58c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**栈不能遍历,不支持随机存取,只能通过 top 从栈顶获取和删除元素.**

> 注意这个是不提供迭代器的,一定得注意

### 栈常用API

#### 构造函数

```c
stack<T> stkT;//stack 采用模板类实现, stack 对象的默认构造形式:
stack(const stack &stk);//拷贝构造函数
```

#### 赋值

```C
stack& operator=(const stack &stk);//重载等号操作符
```

#### 数据存取

```c
push(elem);//向栈顶添加元素
pop();//从栈顶移除第一个元素
top();//返回栈顶元素
```

#### 大小操作

```c
empty();//判断堆栈是否为空
size();//返回堆栈的大小
```



## 队列

### 概述

queue 是一种先进先出(first in first out, FIFO)的数据类型,他有两个口,数据元素只能从一个口进,从另一个口出.队列只允许从队尾加入元素,队头删除元素,必须符合先进先出的原则,queue 和 stack 一样不具有遍历行为。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-abc5901eb4eb2562.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 特性:

- 必须从一个口数据元素入队,另一个口数据元素出队。


- 不能随机存取,不支持遍历

> 注意这个也是不支持迭代器的,一定切记.

### 常用API

#### 构造

```c
queue<T> queT;//queue 采用模板类实现,queue 对象的默认构造形式:
queue(const queue &que);//拷贝构造函数
```

#### 存取

```c
push(elem);//往队尾添加元素
pop();//从队头移除第一个元素
back();//返回最后一个元素
front();//返回第一个元素
```

#### 赋值操作

```c
queue& operator=(const queue &que);//重载等号操作符
```

#### 大小操作

```c
empty();//判断队列是否为空
size();//返回队列的大小
```





## 链表

### 概述

链表是一种物理存储单元上非连续、非顺序的存储结构,数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点(链表中每一个元素称为结点)组成,结点可以在运行时动态生成。每个结点包括两个部分:一个是存储数据元素的数据域,另一个是存储下一个结点地址的指针域。

> 非连续的,添加删除元素都是常熟时间的复杂度,不需要移动元素,效率高.
>
> 链表只有在需要的时候才分配内存
>
> 链表,只要你拿到第一个结点,就相当于整个链表.
>
> 链表需要额外的空间保存结点关系,前驱和后继关系



![image.png](https://upload-images.jianshu.io/upload_images/6836439-ed2c6e8437d27dad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/6836439-3c015ca0dd2a7d44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 特点:

- 采用动态存储分配,不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便,修改指针即可,不需要移动大量元素
- 链表灵活,但是空间和时间额外耗费较大

### 常用API

#### 构造

```c
list<T> lstT;//list 采用采用模板类实现,对象的默认构造形式:
list(beg,end);//构造函数将[beg, end)区间中的元素拷贝给本身。
list(n,elem);//构造函数将 n 个 elem 拷贝给本身。
list(const list &lst);//拷贝构造函数。
```

#### 赋值

```c
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将 n 个 elem 拷贝赋值给本身。
list& operator=(const list &lst);//重载等号操作符
swap(lst);//将 lst 与本身的元素互换。
```



#### 插入与删除

```c
push_back(elem);//在容器尾部加入一个元素
pop_back();//删除容器中最后一个元素
push_front(elem);//在容器开头插入一个元素
pop_front();//从容器开头移除第一个元素
insert(pos,elem);//在 pos 位置插 elem 元素的拷贝,返回新数据的位置。
insert(pos,n,elem);//在 pos 位置插入 n 个 elem 数据,无返回值。
insert(pos,beg,end);//在 pos 位置插入[beg,end)区间的数据,无返回值。
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据,返回下一个数据的位置。
erase(pos);//删除 pos 位置的数据,返回下一个数据的位置。
remove(elem);//删除容器中所有与 elem 值匹配的元素。
```

#### 大小操作

```c
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(num);//重新指定容器的长度为 num,若容器变长,则以默认值填充新位置。如果容器变短,则末尾超出容器长度的元素被删除。
resize(num, elem);//重新指定容器的长度为 num,若容器变长,则以 elem 值填充新位置。如果容器变短,则末尾超出容器长度的元素被删除。
```

#### 存取

```c
front();//返回第一个元素。
back();//返回最后一个元素。
```

#### 反转

```c
reverse();//反转链表,比如 lst 包含 1,3,5 元素,运行此方法后,lst 就包含 5,3,1 元素。
sort(); //list 排序
```

> list.sort这个是属于list类的成员函数.

> 链表和数组有什么区别?
> 1) 数组必须事先定义固定的长度(元素个数),不能适应数据动态地增减的情况。当数据增加时,可能超出原先定义的元素个数;当数据减少时,造成内存浪费。
> 2)
> 链表动态地进行存储分配,可以适应数据动态地增减的情况,且可以方便插入、删除数据元素。(数组中插入、删除数据项时,需要移动其它数据项)

例子:

```c
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <list>
using namespace std;

//初始化
void test01(){
	
	list<int> mlist1;
	list<int> mlist2(10,10); //有参构造
	list<int> mlist3(mlist2);//拷贝构造
	list<int> mlist4(mlist2.begin(), mlist2.end());

	for (list<int>::iterator it = mlist5.begin(); it != mlist4.end();it ++){
		cout << *it << " ";
	}
	cout << endl;

}

//list容器插入删除
void test02(){

	list<int> mlist;

	//插入操作
	mlist.push_back(100);
	mlist.push_front(200); 

	mlist.insert(mlist.begin(),300);
	mlist.insert(mlist.end(),400);
	mlist.insert(mlist.end(), 200);

	list<int>::iterator it = mlist.begin();
	it++;
	it++;
	mlist.insert(it, 500);
	mlist.insert(mlist.end(), 200);
	//删除
	//mlist.pop_back();
	//mlist.pop_front();
	//mlist.erase(mlist.begin(), mlist.end()); //mlist.clear();
	mlist.remove(200); //删除匹配所有值

	list<int>::iterator testit = mlist.begin();
	for (int i = 0; i < mlist.size() - 1;i++){
		testit++;
	}

	(*(mlist.end()));

	cout << "------------" << endl;
	cout << (*testit) << endl;
	cout << mlist.back() << endl;
	cout << "------------" << endl;

	//删除所有200 还是删除第一次出现的200
	/*
		for (list<int>::iterator lit = mlist.begin(); lit != mlist.end(); lit++){
		cout << *lit << " ";
		}
		cout << endl;
	*/

	

	
	
}

//赋值操作
void test03(){

	list<int> mlist;
	mlist.assign(10, 10);


	list<int> mlist2;
	mlist2 = mlist;

	mlist2.swap(mlist);

}

//排序 翻转
void test04(){

	list<int> mlist;
	for (int i = 0; i < 10;i++){
		mlist.push_back(i);
	}

	for (list<int>::iterator it = mlist.begin(); it != mlist.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

	//容器元素反转
	mlist.reverse();

	for (list<int>::iterator it = mlist.begin(); it != mlist.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

}

bool mycompare05(int v1,int v2){
	return v1 > v2;
}

//排序
void test05(){

	list<int> mlist;
	mlist.push_back(2);
	mlist.push_back(1);
	mlist.push_back(7);
	mlist.push_back(5);

	for (list<int>::iterator it = mlist.begin(); it != mlist.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

	//排序  对象怎么排序? 默认从小到大
	mlist.sort();


	for (list<int>::iterator it = mlist.begin(); it != mlist.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

	//从大到小
	mlist.sort(mycompare05);

	for (list<int>::iterator it = mlist.begin(); it != mlist.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

	//算法sort 支持可随机访问的


}

int main(void){
	
	//test01();
	//test02();
	//test04();
	test05();



	return 0;
}
```

