## 概念

### 容器

序列容器：随进入容器的时间和地点决定位置

关联式容器：容器中已经有规则，进入容器的元素位置不是由时机和地点决定。

### 迭代器

用来遍历容器元素的指针，对指针的操作基本都可以对迭代器操作。

实际上，迭代器是一个类，这个类封装一个指针，而且实现了大多数操作符的重载。

### 算法

通过有限步骤，解决问题。

100多个实现好了的算法。

![image.png](https://upload-images.jianshu.io/upload_images/6836439-7fa3d29a6cc2e4c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**三者之间的关系**

### 如何分离实现

```c
#include <iostream>
using namespace std;
//算法，负责统计某个元素的个数
int mycount(int *start,int *end,int val)
{
    int num=0;
    while(start!=end)
    {
        if(*start==val)
        num++;
        start++;
    }
    return num;
}
//int main() {
//
//    int arr[] = {0,7,5,4,9,2,0};
//    int *pbegin = arr;//迭代器开始位置
//    int *pend = &(arr[sizeof(arr)/sizeof(int)]);
//
//    int num = mycount(pbegin,pend,0);
//    cout << "the count is"<<num<<endl;
//    return 0;
//}
```

要想容器和算法之间实现共用，算法必须要用容器所提供的迭代器。

## STL

```c
//
// Created by bruce on 18-5-31.
//
#include<iostream>
#include<vector>
#include <algorithm>

using namespace std;

class Person
{
public:
    Person(int age,int id):age(age),id(id){}

public:
    int age;
    int id;
};
void printVector(int val)
{
    cout << val<<" ";
}
void test01()
{
    vector<int> v;//定义一个容器，并且指定这个容器存放的元素类型ing
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);
    v.push_back(40);
    v.push_back(50);
    v.push_back(60);
    //通过stl提供的for_each算法
    //容器提供的迭代器
    vector<int>::iterator pbegin = v.begin();//指向第一个元素
    vector<int>::iterator pend = v.end();//指向容器的结尾，而不是结束元素
    //容器中可能存放基础的数据类型，也可能存在自定义数据类型

    //遍历容器算法，涉及到回调函数
    for_each(pbegin,pend,printVector);
}
void test02()
{
    vector<Person> v;
    Person p1(10,2),p2(20,34);
    v.push_back(p1);
    v.push_back(p2);
    for(vector<Person>::iterator it = v.begin();it!=v.end();it++)
    {
        cout<<(*it).age<<" "<<(*it).id<<endl;
    }

}
int main()
{

//    test01();
    test02();
    return 0;

}
```

> for_each算法需要提供一个回调函数，因为他不知道你的容器里面的类型，从而不知如果打印遍历元素。
>
> 注意迭代器类型：vector<int>::iterator

## string容器

###  string 的特性

说到 string 的特性,就不得不和 char*类型的字符串的对比:

- Char*是一个指针,String 是一个类string 封装了 char*,管理这个字符串,是一个 char*型的容器。

String 封装了很多实用的成员方法
查找 find,拷贝 copy,删除 delete 替换 replace,插入 insert

不用考虑内存释放和越界
string 管理 char*所分配的内存。每一次 string 的复制,取值都由 string 类负责维护,
不用担心复制越界和取值越界等。
string 和 char*可以互相转换吗?如果能,怎么转换呢?
答案是可以转换。string 转 char*通过 string 提供的 c_str()方法。

#### string 构造

```c
string();//创建一个空的字符串 例如: string str;
string(const string& str);//使用一个 string 对象初始化另一个 string 对象
string(const char* s);//使用字符串 s 初始化
string(int n, char c);//使用 n 个字符 c 初始化
//例子:
//默认构造函数
string s1;
//拷贝构造函数
string s2(s1);
string s2 = s1;
//带参数构造函数
char* str = "itcast";
string s3(str);
string s4(10, 'a');
```



#### string 基本赋值操作

```c
string& operator=(const char* s);//char*类型字符串 赋值给当前的字符串
string& operator=(const string &s);//把字符串 s 赋给当前的字符串
string& operator=(char c);//字符赋值给当前的字符串
string& assign(const char *s);//把字符串 s 赋给当前的字符串
string& assign(const char *s, int n);//把字符串 s 的前 n 个字符赋给当前的字符串
string& assign(const string &s);//把字符串 s 赋给当前字符串
string& assign(int n, char c);//用 n 个字符 c 赋给当前字符串
string& assign(const string &s, int start, int n);//将 s 从 start 开始 n 个字符赋值给字符串
```

#### string存取字符操作

```c
char& operator[](int n);//通过[]方式取字符
char& at(int n);//通过 at 方法获取字符
//例子:
string s = "itcast";char c = s[1];
c = s.at(1);
```

>问:string 中存取字符[]和 at 的异同?
>答: 
>
>1 相同,[]和 at 都可以返回第 n 个字符
>2 不同,at 访问越界会抛出异常,[]越界会直接程序会挂掉。

#### 字符串拼接

```c
string& operator+=(const string& str);//重载+=操作符
string& operator+=(const char* str);//重载+=操作符
string& operator+=(const char c);//重载+=操作符
string& append(const char *s);//把字符串 s 连接到当前字符串结尾
string& append(const char *s, int n);//把字符串 s 的前 n 个字符连接到当前字符串结尾
string& append(const string &s);//同 operator+=()
string& append(const string &s, int pos, int n);//把字符串 s 中从 pos 开始的 n 个字符连接到
当前字符串结尾
string& append(int n, char c);//在当前字符串结尾添加 n 个字符 c
```

#### 字符串查找

```c
int find(const string& str, int pos = 0) const; //查找 str 第一次出现位置,从 pos 开始查找
int find(const char* s, int pos = 0) const;
//查找 s 第一次出现位置,从 pos 开始查找
int find(const char* s, int pos, int n) const; //从 pos 位置查找 s 的前 n 个字符第一次位置
int find(const char c, int pos = 0) const;
//查找字符 c 第一次出现位置
int rfind(const string& str, int pos = npos) const;//查找 str 最后一次位置,从 pos 开始查找
int rfind(const char* s, int pos = npos) const;//查找 s 最后一次出现位置,从 pos 开始查找
int rfind(const char* s, int pos, int n) const;//从 pos 查找 s 的前 n 个字符最后一次位置
int rfind(const char c, int pos = 0) const; //查找字符 c 最后一次出现位置
string& replace(int pos, int n, const string& str); //替换从 pos 开始 n 个字符为字符串 str
string& replace(int pos, int n, const char* s); //替换从 pos 开始的 n 个字符为字符串 s
```

#### string 比较

```c
/*
compare 函数在>时返回 1,<时返回 -1,==时返回 0。
比较区分大小写,比较时参考字典顺序,排越前面的越小。
大写的 A 比小写的 a 小。
*/
int compare(const string &s) const;//与字符串 s 比较
int compare(const char *s) const;//与字符串 s 比较
```

#### string子串

```c++
string substr(int pos = 0, int n = npos) const;//返回由 pos 开始的 n 个字符组成的字符串
```

#### string插入和删除操作

```c
string& insert(int pos, const char* s); //插入字符串
string& insert(int pos, const string& str); //插入字符串
string& insert(int pos, int n, char c);//在指定位置插入 n 个字符 c
string& erase(int pos, int n = npos);//删除从 Pos 开始的 n 个字符
```



#### 课后练习

```c
//用户邮箱地址验证
// 1 判断邮箱有效性 是否包含@和. 并且.在@之后
// 2 判断用户输入的用户名中是否包含除了小写字母之外字符(ASCII 范围 97~122)
// 3 判断用户输入的邮箱地址是否正确(zhaosi@itcast.cn)
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<string>
using namespace std;
// 1 判断邮箱有效性 是否包含@和.并且.在@之后
bool Check_Valid(string& email){
int pos1 = email.find("@");
int pos2 = email.find(".");//判断@和.是否存在
if (pos1 == -1 || pos2 == -1){
return false;
}
//判断@在.之前
if (pos1 > pos2){
return false;
}
return true;
}
//2 判断用户输入的用户名中是否包含除了小写字母之外字符(ASCII 范围 97~122)
bool Check_Username(string& email){
int pos = email.find("@");
string username = email.substr(0,pos-1);
for (string::iterator it = username.begin(); it != username.end(); it++){
if (*it < 97 || *it > 122){
return false;
}
}
return true;
}
// 3 判断用户输入的邮箱地址是否正确(zhaosi@itcast.cn)
bool Check_EqualtTo(string& email){
string rightEmail = "zhaosi@itcast.cn";
if (email.compare(rightEmail) != 0){
return false;
}
return true;
}
void testEmail(){
//用户邮箱地址验证
// 1 判断邮箱有效性 是否包含@和. 并且.在@之后
// 2 判断用户输入的用户名中是否包含除了小写字母之外字符(ASCII 范围 97~122)
// 3 判断用户输入的邮箱地址是否正确(zhaosi@itcast.cn)string email;
cout << "请输入您的邮箱:" << endl;
cin >> email;
bool flag = Check_Valid(email);
if (!flag){
cout << "emain 格式不合法!" << endl;
return;
}
flag = Check_Username(email);
if (!flag){
cout << "用户名中包含除小写字母之外的字母!" << endl;
return;
}
flag = Check_EqualtTo(email);
if (!flag){
cout << "邮箱地址不正确!" << endl;
return;
}
cout << "邮箱输入正确!" << endl;
}
int main(){
testEmail();
system("pause");
return EXIT_SUCCESS;
}
```

