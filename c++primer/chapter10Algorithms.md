[TOC]

## 简介

算法可以应用于多种不同的存储和内置的存储，用法一致。

两个头文件

```c
<algorithm>
<numeric>
```

## find操作

在向量中寻找一个特定的值

```c
int val = 42; // value we'll look for
// result will denote the element we want if it's in vec, or vec.cend() if not
auto result = find(vec.cbegin(), vec.cend(), val);
// report the result
cout << "The value " << val
	<< (result == vec.cend()
		? " is not present" : " is present") << endl;
```

在字符串链表中寻找字符串

```c
string val = "a value"; // value we'll look for
// this call to find looks through string elements in a list
auto result = find(1st.cbegin(), 1st.cend(), val);
```

内置数组类型形式.

```c
int ia[] = {27, 210, 12, 47, 109, 83};
int val = 83;
int* result = find(begin(ia), end(ia), val);
```

> 标准库的begin()和end()函数是C++11新标准引入的函数，可以对数组类型进行操作，返回其首尾指针,所谓的尾指针是最后一个元素的再下一个，对标准库容器操作，返回相应迭代器。
>
> 尾指针:one past the last element of that subrange.
>
> 标准库容器的begin()和end()成员函数属于对应类的成员，返回的是对象容器的首尾迭代器。

```c
// search the elements starting from ia[1] up to but not including ia[4]
auto result = find(ia + 1, ia + 4, val);
```

find的六个工作步骤

> 1. It accesses the first element in the sequence.
> 2. It compares that element to the value we want.
> 3. If this element matches the one we want, find returns a value that identifies this element.
> 4. Otherwise, find advances to the next element and repeats steps 2 and 3.
> 5. find must stop when it has reached the end of the sequence.
> 6. If find gets to the end of the sequence, it needs to return a value indicating that the element was not found. This value and the one returned from step 3 must have compatible types.

### exercise:cout 的用法

```c
vector<int> vec;
    for(int i=0;i<10;i++)
        vec.push_back(i);
      for(int i=0;i<10;i++)
        vec.push_back(i);
    cout << "the count of 5 in the vector is :"<<
        count(vec.cbegin(),vec.cend(),5)
        << endl;
```



## first look at the Algorithms

大多数的算法都提供两个参数,来保证输入范围.

### 只读函数

```c
int sum = accumulate(vec.cbegin(), vec.cend(), 0);
//第三个参数是初始值参数.且第三个参数要和数组里面的类型是兼容的.
```

```c
string sum = accumulate(v.cbegin(), v.cend(), string(""));
//全部数组里面的string全部链接.且string是重载了+的符号,可以用accumulate.
```

> 通常对于只读函数,如果不修改容器里面的变量值的话,那么就可以使用cbegin和cend,如果你需要修改元素值那么就需要使用begin和end()call.

```c
// roster2 should have at least as many elements as roster1
equal(roster1.cbegin(), roster1.cend(), roster2.cbegin());
```

> equal假设了第二个容器和第一个容器的数量是一样的.

```c
    char* a1[] = {"str1","str2","str3"};
    char* a2[] = {"str1","str2","str3"};
    cout << equal(begin(a1),end(a1),begin(a2));//返回的是1
```

```c
vector<double> v;
    v.push_back(1.0);
    v.push_back(2.1);
    cout << accumulate(v.cbegin(),v.cend(),0);//输出的是3,直接截断.有点奇怪的了.
```

## write container elements

### fill_n函数

```c
    vector<int> vec(10);
    fill_n(vec.begin(),vec.size(),1);//一定要保证个数是小于等于实质的空间大小的,要不然填充出错.
```

> 指定个数来填充

### fill函数

```c
fill(vec.begin(), vec.begin() + vec.size()/2, 10);
```

> fill函数指定开始和结束来填充.

> Algorithms that write to a destination iterator  assume the destination is large
> enough to hold the number of elements being written.  
>
> 这么一个假定真的很不人性化,所以我们要自己自动进行设置.

### 和back_inserter一起妙用

```c
vector<int> vec;
fill_n(back_inserter(vec),10,0);
  for(int i=0;i<10;i++)
    cout << vec[i] << endl;

auto it = back_inserter(vec); // assigning through it adds elements to vec
*it = 42; // vec now has one element with value 42
```

### copy用法

```c
 int a1[] = {0,1,2,3,4,5,6,7,8,9};
int a2[sizeof(a1)/sizeof(*a1)]; // a2 has the same size as a1
// ret points just past the last element copied into a2
auto ret = copy(begin(a1), end(a1), a2); // copy a1 into a2
    for(int i=0;i<10;i++)
        cout << a2[i] << endl;
```

### replace

```c
// replace any element with the value 0 with 42
replace(ilst.begin(), ilst.end(), 0, 42);
```

### replace_copy

```c
// use back_inserter to grow destination as needed
replace_copy(ilst.cbegin(), ilst.cend(),
back_inserter(ivec), 0, 42);
```

> After this call, ilst is unchanged, and ivec contains a copy of ilst with the
> exception that every element in ilst with the value 0 has the value 42 in ivec.

### eliminating Duplicates

```c
void elimDups(vector<string> &words)
{
// sort words alphabetically so we can find the duplicates
sort(words.begin(), words.end());
// unique reorders the input range so that each word appears once in the
// front portion of the range and returns an iterator one past the unique range
auto end_unique = unique(words.begin(), words.end());
// erase uses a vector operation to remove the nonunique elements
words.erase(end_unique, words.end());
}
```

