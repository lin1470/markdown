### 特性

set/multiset 的特性是所有元素会根据元素的值自动进行排序。set 是以 RB-tree(红黑树,平衡二叉树的一种)为底层机制,其查找效率非常好。set 容器中不允许重复元
素,multiset 允许重复元素。
二叉树就是任何节点最多只允许有两个字节点。分别是左子结点和右子节点。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-6ef1e7f14fc5eb1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

二叉搜索树,是指二叉树中的节点按照一定的规则进行排序,使得对二叉树中元素访问更加高效。二叉搜索树的放置规则是:任何节点的元素值一定大于其左子树中的每一个节点的元素值,并且小于其右子树的值。因此从根节点一直向左走,一直到无路可走,即得到最小值,一直向右走,直至无路可走,可得到最大值。那么在儿茶搜索树中找到最大元素和最小元素是非常简单的事情。下图为二叉搜索树:

![image.png](https://upload-images.jianshu.io/upload_images/6836439-3021a5437f5c8f59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面我们介绍了二叉搜索树,那么当一个二叉搜索树的左子树和右子树不平衡的时候,那么搜索依据上图表示,搜索 9 所花费的时间要比搜索 17 所花费的时间要多,由于我们的输入或者经过我们插入或者删除操作,二叉树失去平衡,造成搜索效率降低。所以我们有了一个平衡二叉树的概念,所谓的平衡不是指的完全平衡。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-72191b2712ada494.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 问:我们可以通过 set 的迭代器改变元素的值吗?
> 答: 不行,因为 set 集合是根据元素值进行排序,关系到 set 的排序规则,如果任意改变 set 的元素值,会严重破坏 set 组织。
>
> **只能用insert插入的时候也会自动进行排序**

### 常用API

#### 构造

```c
set<T> st;//set 默认构造函数:
mulitset<T> mst; //multiset 默认构造函数:
set(const set &st);//拷贝构造函数
```

#### 赋值

```c
set& operator=(const set &st);//重载等号操作符
swap(st);//交换两个集合容器
```

#### set 大小操作

```c
size();//返回容器中元素的数目
empty();//判断容器是否为空
```

#### 插入和删除

```c
insert(elem);//在容器中插入元素。
clear();//清除所有元素
erase(pos);//删除 pos 迭代器所指的元素,返回下一个元素的迭代器。
erase(beg, end);//删除区间[beg,end)的所有元素 ,返回下一个元素的迭代器。
erase(elem);//删除容器中值为 elem 的元素。
```

#### 查找(重点)

```c
find(key);//查找键 key 是否存在,若存在,返回该键的元素的迭代器;若不存在,返回 map.end();
lower_bound(keyElem);//返回第一个 key>=keyElem 元素的迭代器。
upper_bound(keyElem);//返回第一个 key>keyElem 元素的迭代器。
equal_range(keyElem);//返回容器中 key 与 keyElem 相等的上下限的两个迭代器。
```



> 若要实现自定义的排序,则要实现仿函数,其实就是一个类
>
> ```c
> //仿函数
> class mycompare{
> public:
> 	bool operator()(int v1,int v2){
> 		return v1 > v2;
> 	}
> };
> ```



例子:

```c
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <set>
#include <list>
using namespace std;

//仿函数
class mycompare{
public:
	bool operator()(int v1,int v2){
		return v1 > v2;
	}
};

//set容器初始化
void test01(){

	
	set<int, mycompare> s1;  //自动进行排序 默认从小到大
	s1.insert(7);
	s1.insert(2);
	s1.insert(4);
	s1.insert(5);
	s1.insert(1);

	for (set<int>::iterator it = s1.begin(); it != s1.end();it ++){
		cout << *it << " ";
	}
	cout << endl;
#if 0
	//赋值操作
	set<int> s2;
	s2 = s1;

	//删除操作
	s1.erase(s1.begin()); 
	s1.erase(7);

	for (set<int>::iterator it = s1.begin(); it != s1.end(); it++){
		cout << *it << " ";
	}
	cout << endl;

	//先序遍历 中序遍历 后序遍历
	//如何改变默认排序？
#endif
}



//set查找
void test02(){
	
	set<int> s1;
	s1.insert(7);
	s1.insert(2);
	s1.insert(4);
	s1.insert(5);
	s1.insert(1);

	set<int>::iterator ret = s1.find(14);
	if (ret == s1.end()){
		cout << "没有找到!" << endl;
	}
	else{
		cout << "ret:" << *ret << endl;
	}

	//找第一个大于等于key的元素
	ret = s1.lower_bound(2); 
	if (ret == s1.end()){
		cout << "没有找到!" << endl;
	}
	else{
		cout << "ret:" << *ret << endl;
	}

	//找第一个大于key的值
	ret = s1.upper_bound(2);
	if (ret == s1.end()){
		cout << "没有找到!" << endl;
	}
	else{
		cout << "ret:" << *ret << endl;
	}

	//equal_range 返回Lower_bound 和 upper_bound值
	pair<set<int>::iterator,set<int>::iterator> myret = s1.equal_range(2);
	if (myret.first == s1.end()){
		cout << "没有找到！" << endl;
	}
	else{
		cout << "myret:" << *(myret.first) << endl;
	}

	if (myret.second == s1.end()){
		cout << "没有找到！" << endl;
	}
	else{
		cout << "myret:" << *(myret.second) << endl;
	}

}

class Person{
public:
	Person(int age,int id):id(id),age(age){}
public:
	int id;
	int age;
};

class mycompare2{
public:
	bool operator()(Person p1,Person p2){
		if (p1.id == p2.id){
			return p1.age > p2.age;
		}
		else{
			return p1.id > p2.id;
		}
	}
};

void test03(){

	set<Person, mycompare2> sp; //set需要排序，当你放对象，set知道怎么排吗？

	Person p1(10, 20), p2(20, 20), p3(50, 60);
	sp.insert(p1);
	sp.insert(p2);
	sp.insert(p3);

	Person p4(10, 30);

	for (set<Person, mycompare2>::iterator it = sp.begin(); it != sp.end();it++){
		cout << (*it).age << " " << (*it).id << endl;
	}

	//查找
	set<Person, mycompare2>::iterator ret =  sp.find(p4);
	if (ret == sp.end()){
		cout << "没有找到!" << endl;
	}
	else{
		cout << "找到：" << (*ret).id << " " << (*ret).age << endl;
	}
}

int main(void){

	//test01();
	//test02();
	test03();


	
	return 0;
}
```



### 对象排序问题

如果放的是一个对象的话,那么需要指定如何确定前后关系.这个也是需要用仿函数来实现前后序号.

> 用什么属性来实现排序那么改属性就是关键字.查找也是基于这个关键字的,返回的是一个迭代器.





## 对组



对组(pair)将一对值组合成一个值,这一对值可以具有不同的数据类型,两个值可以分
别用 pair 的两个公有函数 first 和 second 访问。
类模板:template <class T1, class T2> struct pair.

### 常用API

#### 构造

```c
//第一种方法创建一个对组
pair<string, int> pair1(string("name"), 20);
cout << pair1.first << endl; //访问 pair 第一个值
cout << pair1.second << endl;//访问 pair 第二个值
//第二种pair<string, int> pair2 = make_pair("name", 30);
cout << pair2.first << endl;
cout << pair2.second << endl;
//pair=赋值
pair<string, int> pair3 = pair2;
cout << pair3.first << endl;
cout << pair3.second << endl;
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <string>
using namespace std;

void test01(){

	//构造方法
	pair<int, int> pair1(10,20);
	cout << pair1.first << " " << pair1.second << endl;

	pair<int, string> pair2 = make_pair(10, "aaa");
	cout << pair2.first << " " << pair2.second << endl;

	pair<int, string> pair3 = pair2;
}

int main(void){
	test01();
	return 0;
}
```



## map

### 特点

map 相对于 set 区别,map 具有键值和实值,所有元素根据键值自动排序。pair 的第
一元素被称为键值,第二元素被称为实值。map 也是以红黑树为底层实现机制。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-c6e3fbf87cb33a57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 问题 : 我们通过 map 的迭代器可以修改 map 的键值吗?
> 答案是否定的,键值关系到容器内元素的排列规则,任意改变键值会破坏容器的排列规则,但是你可以改变实值。
> **map 和 multimap 区别在于,map 不允许相同 key 值存在,multimap 则允许相同key 值存在。**

### 常用API

#### 构造

```c
map<T1, T2> mapTT;//map 默认构造函数:
map(const map &mp);//拷贝构造函数
```

#### 赋值操作

```c
map& operator=(const map &mp);//重载等号操作符
swap(mp);//交换两个集合容器
```

#### 大小

```c
size();//返回容器中元素的数目
empty();//判断容器是否为空
```

#### 插入

```c
map.insert(...); //往容器插入元素,返回 pair<iterator,bool>
map<int, string> mapStu;
// 第一种 通过 pair 的方式插入对象
mapStu.insert(pair<int, string>(3, "小张"));
// 第二种 通过 pair 的方式插入对象
mapStu.inset(make_pair(-1, "校长"));
// 第三种 通过 value_type 的方式插入对象
mapStu.insert(map<int, string>::value_type(1, "小李"));
// 第四种 通过数组的方式插入值
mapStu[3] = "小刘";
mapStu[5] = "小王";
```

> - 前三种方法,采用的是 insert()方法,该方法返回值为 pair<iterator,bool>
> - 第四种方法非常直观,但存在一个性能的问题。插入 3 时,先在mapStu 中查找主键为 3 的项,若没发现,则将一个键为 3,值为初始化值的对组插入到 mapStu 中,然后再将值修改成“小刘”。若发现已存在 3 这个键,则修改这个键对应的 value。
>
>
> - string strName = mapStu[2];//取操作或插入操作
>
> 只有当 mapStu 存在 2 这个键时才是正确的取操作,否则会自动插入一个实例, 键为 2,值为初始化值。



#### 查找

```c
find(key);//查找键 key 是否存在,若存在,返回该键的元素的迭代器;/若不存在,返回 map.end();
count(keyElem);//返回容器中 key 为 keyElem 的对组个数。对 map 来说,要么是 0,要么是 1。对
multimap 来说,值可能大于 1。
lower_bound(keyElem);//返回第一个 key<=keyElem 元素的迭代器。
upper_bound(keyElem);//返回第一个 key>keyElem 元素的迭代器。
equal_range(keyElem);//返回容器中 key 与 keyElem 相等的上下限的两个迭代器
```



例子:

```c
//
// Created by bruce on 18-6-9.
//

#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <map>
using namespace std;

//map容器初始化
void test01(){

    //map容器模板参数，第一个参数key的类型，第二参数value类型
    map<int, int> mymap;

    //插入数据  pair.first key值 piar.second value值
    //第一种
    pair<map<int, int>::iterator, bool> ret = mymap.insert(pair<int, int>(10, 10));
    if (ret.second){
        cout << "第一次插入成功!" << endl;
    }
    else{
        cout << "插入失败!" << endl;
    }
    ret = mymap.insert(pair<int, int>(10, 20));
    if (ret.second){
        cout << "第二次插入成功!" << endl;
    }
    else{
        cout << "插入失败!" << endl;
    }
    //第二种
    mymap.insert(make_pair(20, 20));
    //第三种
    mymap.insert(map<int, int>::value_type(30,30));
    //第四种
    mymap[40] = 40;
    mymap[10] = 20;
    mymap[50] = 50;
    //发现如果key不存在，创建pair插入到map容器中
    //如果发现key存在，那么会修改key对应的value

    //打印
    for (map<int, int>::iterator it = mymap.begin(); it != mymap.end();it ++){
        // *it 取出来的是一个pair
        cout << "key:" << (*it).first << " value:" << it->second << endl;
    }

    //如果通过【】方式去访问map中一个不存在key，
    //那么map会将这个访问的key插入到map中,并且给value一个默认值
    cout << " mymap[60]: " <<  mymap[60] << endl;

    //打印
    for (map<int, int>::iterator it = mymap.begin(); it != mymap.end(); it++){
        // *it 取出来的是一个pair
        cout << "key:" << (*it).first << " value:" << it->second << endl;
    }

}

class MyKey{
public:
    MyKey(int index,int id){
        this->mIndex = index;
        this->mID = id;
    }
public:
    int mIndex;
    int mID;
};

struct mycompare{ //仿函数,来自定义排序的顺序
    bool operator()(MyKey key1, MyKey key2){
        return key1.mIndex > key2.mIndex;
    }
};

void test02(){

    map<MyKey, int, mycompare> mymap; //自动排序，自定数据类型，咋排?

    mymap.insert(make_pair(MyKey(1, 2), 10));
    mymap.insert(make_pair(MyKey(4, 5), 20));

    for (map<MyKey, int, mycompare>::iterator it = mymap.begin(); it != mymap.end();it ++){
        cout << it->first.mIndex << ":" << it->first.mID << " = " << it->second << endl;
    }
}

//equal_range
void test03(){

    map<int, int> mymap;
    mymap.insert(make_pair(1, 4));
    mymap.insert(make_pair(2, 5));
    mymap.insert(make_pair(3, 6));

    pair<map<int, int>::iterator, map<int, int>::iterator> ret =  mymap.equal_range(2);
    if (ret.first != mymap.end()){
        cout << "找到lower_bound！" << endl;
    }
    else{
        cout << "没有找到";
    }

    if (ret.second != mymap.end()){
        cout << "找到upper_bound！" << endl;
    }
    else{
        cout << "没有找到";
    }



}

int main(void){

    //test01();
    //test02();
    test03();

    return 0;
}
```





