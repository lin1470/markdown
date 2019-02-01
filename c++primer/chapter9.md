[TOC]

## 顺序容器

![image.png](https://upload-images.jianshu.io/upload_images/6836439-bbf5a55b544d5186.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> string和vector将元素保存在连续的内存空间中。由于元素是连续存储的，由元素的下标来计算其地址是非常快速的。但是在中间位置添加火删除元素非常耗时：在插入火删除位置要移动所有元素。

> list和forward_list两个容器就是添加和删除都很快，但是不支持随机访问。而且开销很大

> deque支持快速随机访问。删除添加代价高。但deque的两端添加或删除元素快。

**现代c++程序应该使用标准库容器，而不是更原始的数据结构，如内置数组**

![image.png](https://upload-images.jianshu.io/upload_images/6836439-2c49727d0f699109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为什么链表的中间插入或者删除快呢，就是因为他是链表，不是连续的空间内存，只需要找到位置，然后新建内存或者删除节点就行了。

## 迭代器

### 迭代器范围

[begin,end)

注意这个是不包括后面的一个函数的.

### 迭代器类型

vector<int>::iterator 

当不需要写访问时一般使用cbegin和cend

## 容器操作

### 初始化

```
v1[10] = {0,1,0,0,3,0,0,4,4,4}
```

直接百度,简单

> 将一个容器初始化为另一个容器的拷贝时,两个容器的容器类型和元素类型都必须相同
>
> ```c
> list<string> arutors = {"sdfdf","sdfsdf"};
> vector<sring> sdf(arutors); //明显不一样
> list<string> aaa(arutors);//这个倒是可以的.
> ```

### 赋值和swap

赋值一般使用assign

```
array<int,10> a1 = {0,1,2,3,4,````};//是个元素
array<int,10> a2 = {0};//全部为0
a1 = a2; //替换a1
a2 = {0}; //错误

```

```
list<string> names;
vector<string> oldstyle;
names = oldstyle; //错误
names.assign(oldstyle.cbegin(),oldstyle.cend());
```

所以一般不同类型的赋值,一般都是用迭代器的形式进行.

#### swap

vector

![](https://ws1.sinaimg.cn/large/891f7782gy1fwgviihlc9j20et051dgc.jpg)

明显改变的元素里面的值,可以简单地改变指针的值

array



![](https://ws1.sinaimg.cn/large/891f7782gy1fwgvjs9dgyj20ft05dmxm.jpg)

改变的是地址里面的值.

> 和其它容器不同的是，对string调用swap会导致迭代器、引用和指针失效。因为string存储的是字符串，在string变量中真正存储字符串的是一个叫_Ptr的指针，它指向string所存储的字符串首地址，而字符串并没有固定地址，而是存储在一个临时内存区域中，所以当字符串发生改变时，会发生内存的重新分配，所以会导致迭代器、引用和指针失效.

### 添加

push_back 添加到后面(除了array,K和forward_list)

push_front (list,forward_list,deque都支持)

insert(vector,deque,list都支持)

> insert(iter,"hello")在iter后面插入
>
> insert(svec.end(),10,"anna"); //范围插入
>
> insert(slist.begin(),v.end()-1,v.end()); // 注意最后面一个都是范围的值

#### emplace_front,emplace和emplace_back

emplace 函数在容器中直接构造元素,传递给emplace函数的参数必须和元素类型的构造函数相匹配

```
 vector<Person> pvec;
    pvec.emplace_back(2,"hello"); //使用构造函数
    pvec.emplace(pvec.begin());  // 指定 位置,使用默认构造函数
    pvec.emplace(pvec.begin(),1,"hello"); // 使用构造函数
    for(int i=0;i!=pvec.size();i++)
    {
        cout << pvec.at(i).id << pvec.at(i).name<< endl;
    }
```



### 调整大小

resize() 调整大小

```
list<int> ilist(10,42);
ilist.resize(15); // 将5个0插入末尾
ilist.resize(25,-1)// 插入10个-1
ilist.resize(5) //删除多的20个元素
```

### 删除

自行百度

一般使用erase(it,,) // 删除指定函数

clear()删除所有元素

### 迭代器失效

![](https://ws1.sinaimg.cn/large/891f7782gy1fwh2uapld2j20kc0jg12r.jpg)

> 总之就是删除或者添加元素之后迭代器都会失效,因此建议每次操作之后都要重新定位迭代器.这个对vector,string和deque尤其重要

insert 会在指定之前插入,返回指向插入的元素

erase 会返回删除之后的下一个

```c++
vector<int> vi = {0,1,2,3,4,5,6,7,8,9};
    auto iter = vi.begin();
    while(iter != vi.end())
    {
        if(*iter %2)
        {
            iter = vi.insert(iter,*iter);
            iter += 2;
        }
        else
        {
            iter = vi.erase(iter);
        }
    }

    for(auto it= vi.cbegin();it != vi.cend();it++)
    {
        cout << *it << endl;
    }
```

> 最后不要用变量保存end返回的迭代器,因为插入一些元素之后会使得变量保存的迭代器失效.应当每次使用的时候都要用end().

### vector 对象如何增长

> vector中的capacity是容量,而size是保存的元素的个数.
>
> reserve是改变的是容量,resize改变的是size
>
> shrink_to_fit是使得size和capacity相同.

**有个问题:就是为什么需要capacity容量这个**

> 因为在设计的时候,总是预留有多的容量空间来装载元素,当元素个数比容量大时,这时候才分配新的内存空间来保存新的内容,这样就能够保证不必每次删除或者添加都要重新分配内存,加快操作的速率.

## string

### 构造

直接百度吧

### substr

```
s.substr(pos,n)返回一个string,包含s中从pos开始的n个字符的拷贝.pos的默认值为0,n的默认值是s.size()-pos,即拷贝从pos开始的所有字符
```

### 操作

- assign

- insert
- erase
- append
- replace

#### 搜索

- find
- rfind
- find_first_of
- find_last_of
- find_first_not_of
- find_last_not_of

#### 比较

- compare

#### 数值转换

- to_string
- stoi
- stol
- stoul
- stoll
- stoull
- stof
- stod
- stold

## 容器适配器

### stack

操作百度(下同)

### queue

### priority_queue(优先队列)