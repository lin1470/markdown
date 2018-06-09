[TOC]

## 概述

vector容器

动态数组, 可变数组 vector容器,单口容器

![image.png](https://upload-images.jianshu.io/upload_images/6836439-e6ed364eff2987dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

rend和rbegin与begin 和end相对应

因为是连续的空间,所以插入很慢,效率低,虽然提供了insert操作但是不建议,尽量只在尾部进行操作

![image.png](https://upload-images.jianshu.io/upload_images/6836439-8562a8a29ce22231.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个好像就是C语言数据结构中的动态数组的实现.



## 常用API

### 构造函数

```c
vector<T> v; //采用模板实现类实现,默认构造函数
vector(v.begin(), v.end());//将 v[begin(), end())区间中的元素拷贝给本身。
vector(n, elem);//构造函数将 n 个 elem 拷贝给本身。
vector(const vector &vec);//拷贝构造函数。
//例子 使用第二个构造函数 我们可以...
int arr[] = {2,3,4,1,9};
vector<int> v1(arr, arr + sizeof(arr) / sizeof(int));
```

### 常用赋值操作

```c
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将 n 个 elem 拷贝赋值给本身。
vector& operator=(const vector
&vec);//重载等号操作符
swap(vec);// 将 vec 与本身的元素互换。
//第一个赋值函数,可以这么写:
int arr[] = { 0, 1, 2, 3, 4 };
assign(arr, arr + 5);//使用数组初始化 vector
```

### 大小操作

```c
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(int num);//重新指定容器的长度为 num,若容器变长,则以默认值填充新位置。如果容器变
短,则末尾超出容器长度的元素被删除。
resize(int num, elem);//重新指定容器的长度为 num,若容器变长,则以 elem 值填充新位置。如
果容器变短,则末尾超出容器长>度的元素被删除。
capacity();//容器的容量
reserve(int len);//容器预留 len 个元素长度,预留位置不初始化,元素不可访问。
```

>注意: resize 若容器变长,则以默认值填充新位置。如果容器变短,则末尾超出容器长度的
>元素被删除。

> 问:reserv 和 resize 的区别?
>
> 答:reserve 是容器预留空间,但在空间内不真正创建元素对象,所以在没有添加新的对象之前,不能引用容器内的元素.resize 是改变容器的大小,且在创建对象,因此,调用这个函数之后,就可以引用容器内的对象了.

> 如果大概知道了数据的个数,那么就可以直接用reserve来预留空间,这样可以大大减少扩容的个数.

```c
vector<int> v;
int* p = NULL;
int count = 0;// 统计 vector 容量增长几次?
for (int i = 0; i < 100000;i++){
v.push_back(i);
if (p != &v[0]){
p = &v[0];
count++;
}
}
cout << "count:" << count << endl; //打印出 30

vector<int> v;
v.reserve(100000);
int* p = NULL;
int count = 0;// 统计 vector 容量增长几次?
for (int i = 0; i < 100000;i++){
v.push_back(i);
if (p != &v[0]){
p = &v[0];
count++;
}
}
cout << "count:" << count << endl; //打印出 30

```

### 数据存取操作

```c
at(int idx); //返回索引 idx 所指的数据,如果 idx 越界,抛出 out_of_range 异常。
operator[];//返回索引 idx 所指的数据,越界时,运行直接报错
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素
```

### 插入和删除操作

```c
insert(const_iterator pos, int count,ele);//迭代器指向位置 pos 插入 count 个元素 ele.
push_back(ele); //尾部插入元素 ele
pop_back();//删除最后一个元素
erase(const_iterator start, const_iterator end);//删除迭代器从 start 到 end 之间的元素
erase(const_iterator pos);//删除迭代器指向的元素
clear();//删除容器中所有元素
```

```c
//
// Created by bruce on 18-6-7.
//
#include<iostream>
#include<vector>
using namespace std;
void printVector(vector<int> &v)
{
    for(vector<int>::iterator it = v.begin();it!= v.end();it++)
    {
        cout << *it<< " ";
    }
    cout << endl;
}
void test01()
{
    vector<int> v1; //默认构造
    int arr[] = {10,20,30,40};
    vector<int> v2(arr,arr+sizeof(arr)/sizeof(int));
    vector<int> v3(v2.begin(),v2.end());
    vector<int> v4(v3);
    printVector(v2);
    printVector(v3);
    printVector(v4);
}

void test02()
{
    int arr[] = {10,20,30,40};
    vector<int> v1(arr,arr+sizeof(arr)/sizeof(int));
    //成员方法
    vector<int> v2;
    v2.assign(v1.begin(),v1.end());
    //重载
    vector<int> v3;
    v3 = v2;

    printVector(v1);
    printVector(v2);

    cout << " swap" << endl;

    int arr1[] = {100,200,300,400,500};
    vector<int> v4(arr1,arr1+sizeof(arr1)/sizeof(int));
    v4.swap(v1);
    //这个swap的原理不过就是交换指针而已
    //果然,因为这个两个数组的长度不一样,如果是真真交换数据的话,那么应该会出现错误.

    printVector(v1);
    printVector(v2);
    printVector(v4);
}

void test03()
{
    int arr[] = {10,20,30,40};
    vector<int> v1(arr,arr+sizeof(arr)/sizeof(int));
    cout << "size: "<<v1.size() << endl;

    if(v1.empty()){
        cout << "empty\n";
    }
    else cout << "not empty\n";

    cout << "capacity" << v1.capacity() << endl;

    vector<int> v;
    int* p = NULL;
    int count = 0;// 统计 vector 容量增长几次?
    for (int i = 0; i < 100000;i++){
        v.push_back(i);
        if (p != &v[0]){
            p = &v[0];
            count++;
        }
    }
    cout << "count:" << count << endl; //打印出 30

    vector<int> v2;
    v.reserve(100000);
    for (int i = 0; i < 100000;i++){
        v.push_back(i);
        if (p != &v2[0]){
            p = &v2[0];
            count++;
        }
    }
    cout << "count:" << count << endl; //打印出 30
}


void test04()
{
    int arr[] = {10,20,30,40};
    vector<int> v4(arr,arr+sizeof(arr)/sizeof(int));

    for(int i =0 ;i<v4.size();i++)
    {
        cout << v4[i] << " ";
    }
    cout << endl;

    v4.at(0) = 0;
    for(int i =0;i<v4.size();i++)
        cout << v4.at(i) << " ";

}
int main()
{
//    test01();
//    test02();
    test03();
//    test04();
    return 0;
}
```



### 总结:

> vector 是个动态数组,当空间不足的时候插入新元素,vector 会重新申请一块更大的内存空间,将旧空间数据拷贝到新空间,然后释放旧空间。vector 是单口容器,所以在尾端插入和删除元素效率较高,在指定位置插入,势必会引起数据元素移动,效率较低。