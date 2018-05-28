[TOC]



## 6.1Funciton Basics 

**Parameters and Arguments**

The number and type of the Arguments must match the corresponding parameter in the function calling .

```c++ 
fact("hello"); // error
fact(); // error
fact(42,10,0); // error :too many arguments 
fact(3.14); // ok:arguments is converted to int, equivalent to fact(3) ;     
```

**funciton Parameter List **

A function's paramenter list can be empty but cannot be omitted . typically we define a function with no parameters by writing an empty parameter list . We can use the keyword void to indicate that there are no parameters 

```c++ 
void f1(){
 //***   
}
void f2(void){//****}
```

parameter 是形参,argument是实参.

**Funciton Return Type**

**The return type may not be an array type or a function type .However , a funcion may return a pointer to an array or a function .**

**Automatic Ojects **

Objects that exist only while a block is executing are known as automatic objects .

**local static object **

Local statics are not destroyed when a function ends; they are destroyed when the program terminates.

```c++ 
size_t count_calls()
{
    static size_t ctr = 0; // value will persist across calls
    return ++ctr;
}
int main()
{
    for (size_t i = 0; i != 10; ++i)
    cout << count_calls() << endl;
    return 0;
}
```

if a local static has no explicit initializer ,it's value initialized meaning that local statics of built-in type are initialized to zero .

## 6.2Argument Passing 

if the parameter is a reference , then the parameter is bound to its argument. Otherwise , the arguments value is copied .

### Passing Arguments by Value 

like this 

```c++
int n = 0;
int i =n ;
i =42 // value in i is changed; n is unchanged.
```

**Pointer Parameters**

The pointer is copied. After the copy , the two pointers are distinct .

```c++
void reset(int *ip)
{
    *ip = 0; // changes the value of the object to which ip points
    ip = 0; // changes only the local copy of ip; the argument is unchanged
}

int i = 42;
reset(&i); // changes i but not the address of i
cout << "i = " << i << endl; // prints i = 0
```

### Passing Value By Reference 

It can be inefficient to copy objects of large class types or large containers . moreover,some class types (including the IOn types ) cannot be copied .Functions must use reference parameters to operate on objects of a type that cannot be copied .

if we will not change the arguments ,we can make the parameters references to const 

```c++ 
bool isShorter(const string &s1, const string &s2)
{
	return s1.size() < s2.size();
}
```

**Using Reference Parameters to Return Additional Information**

```c++
// returns the index of the first occurrence of c in s
// the reference parameter occurs counts how often c occurs
string::size_type find_char(const string &s, char c,string::size_type &occurs)
{
    auto ret = s.size(); // position of the first occurrence, if any
    occurs = 0; // set the occurrence count parameter
    for (decltype(ret) i = 0; i != s.size(); ++i) {
    if (s[i] == c) {
        if (ret == s.size())
        ret = i; // remember the first occurrence of c
        ++occurs; // increment the occurrence count
		}
	}
	return ret; // count is returned implicitly in occurs
}
```

we can call find_char as follows:

```c++
auto index = find_char(s,'0',ctr);
```

### Array Parameters 

```c++
void print(const int*);
void print(const int[]); // shows the intent that the function takes an array
void print(const int[10]); // dimension for documentation purposes (atbest)

int i = 0, j[2] = {0, 1};
print(&i); // ok: &i is int*
print(j); // ok: j is converted to an int* that points to j[0]
```

**using a Marker to Specify the Extent of an Array **

```c++ 
void print(const char *cp)
{
if (cp) // if cp is not a null pointer
    while (*cp) // so long as the character it points to is not a null character
   	 	cout << *cp++; // print the character and advance the pointer
}
```

**Using the Standard Library Conventions**

```c++ 
void print(const int *beg, const int *end)
{
	// print every element starting at beg up to but not including end
    while (beg != end)
        cout << *beg++ << endl; // print the current element
    // and advance the pointer
}

int j[2] = {0,1};
print(begin(j),end(j));
```

**explicitly Passing a Size Parameter **

```c++ 
void print(const int ia[], size_t size)
{
    for (size_t i = 0; i != size; ++i) {
    	cout << ia[i] << endl;
    }
}
```

##   6.4Overloaded Functions

**same name but different parameter lists in the same scope** 

such as 

```c++ 
void print(const char *cp);
void print(const int *beg, const int *end);
void print(const int ia[], size_t size);
```

**main function may not be overloaded .**

```c++ 
Record lookup(const Account&); // find by Account
Record lookup(const Phone&); // find by Phone
Record lookup(const Name&); // find by Name
Account acct;
Phone phone;
Record r1 = lookup(acct); // call version that takes an Account
Record r2 = lookup(phone); // call version that takes a Phone
```

**It is an error for two functions to differ only in terms of their return types. If the parameter lists of two functions match but the return types differ, then the second declaration is an error:**

```c++ 
Record lookup(const Account&);
bool lookup(const Account&); // error: only the return type is different
```

### Overloading and Scope 

**Ordinarily, it is a bad idea to declare a function locally. However, to explain how scope interacts with overloading, we will violate this practice and use**
**local function declarations.**

```c++ 
string read();
void print(const string &);
void print(double); // overloads the print function
void fooBar(int ival)
{
    bool read = false; // new scope: hides the outer declaration of read
    string s = read(); // error: read is a bool variable, not a function
    // bad practice: usually it's a bad idea to declare functions at local scope
    void print(int); // new scope: hides previous instances of print
    print("Value: "); // error: print(const string &) is hidden
    print(ival); // ok: print(int) is visible
    print(3.14); // ok: calls print(int); print(double) is hidden
}
```

Exactly the same process is used to resolve the calls to print. The declaration of
print(int) in fooBar hides the earlier declarations of print. It is as if there isonly one  print function available: the one that takes a single int parameter.

When we call print, the compiler first looks for a declaration of that name. It finds
the local declaration for print that takes an int. Once a name is found, the compiler
ignores uses of that name in any outer scope. Instead, the compiler assumes that the declaration it found is the one for the name we are using. What remains is to see if the use of the name is valid.

```c++ 
void print(const string &);
void print(double); // overloads the print function
void print(int); // another overloaded instance
void fooBar2(int ival)
{
    print("Value: "); // calls print(const string &)
    print(ival); // calls print(int)
    print(3.14); // calls print(double)
}
```

## 6.5Features for Specialized Uses 

**Arguments in the call are resolved by position. The default arguments are used for the trailing (right-most) arguments of a call. For example, to override the default for background, we must also supply arguments for height and width:**

```c++ 
window = screen(, , '?'); // error: can omit only trailing arguments
window = screen('?'); // calls screen('?',80,' ')
```

**inline mechanism i**

I**n general, the inline mechanism is meant to optimize small, straight-line functions that are called frequently. Many compilers will not inline a recursive function. A 75-line function will almost surely not be expanded inline.**

## 6.7pointers to Functions 

 Like any other pointer, a function pointer points to a particular type. A
function’s type is determined by its return type and the types of its parameters. The
function’s name is not part of its type. For example:

```c++ 
// compares lengths of two strings
bool lengthCompare(const string &, const string &);
```

has type bool(const string&, const string&). To declare a pointer that can
point at this function, we declare a pointer in place of the function name:

```c++ 
// pf points to a function returning bool that takes two const string references
bool (*pf)(const string &, const string &); // uninitialized
```

Starting from the name we are declaring, we see that pf is preceded by a *, so pf is
a pointer. To the right is a parameter list, which means that pf points to a function.
Looking left, we find that the type the function returns is bool. Thus, pf points to a
function that has two const string& parameters and returns bool.

> The parentheses around *pf are necessary. If we omit the parentheses, then we declare pf as a function that returns a pointer to bool:
> Click here to view code image
> // declares a function named pf that returns a bool*
> bool *pf(const string &, const string &);

```c++ 
bool b1 = pf("hello", "goodbye"); // calls lengthCompare
bool b2 = (*pf)("hello", "goodbye"); // equivalent call
bool b3 = lengthCompare("hello", "goodbye"); // equivalent call
```

There is no conversion between pointers to one function type and pointers to
another function type. However, as usual, we can assign nullptr (§ 2.3.2, p. 53) or
a zero-valued integer constant expression to a function pointer to indicate that the
pointer does not point to any function:

```c++ 
string::size_type sumLength(const string&, const string&);
bool cstringCompare(const char*, const char*);
pf = 0; // ok: pf points to no function
pf = sumLength; // error: return type differs
pf = cstringCompare; // error: parameter types differ
pf = lengthCompare; // ok: function and pointer types match exactly
```

**Pointers to Overloaded Functions**

As usual, when we use an overloaded function, the context must make it clear which version is being used. When we declare a pointer to an overloaded function

```c++
void ff(int*);
void ff(unsigned int);
void (*pf1)(unsigned int) = ff; // pf1 points to ff(unsigned)
```

the compiler uses the type of the pointer to determine which overloaded function to
use. The type of the pointer must match one of the overloaded functions exactly:```

```c++ 
void (*pf2)(int) = ff; // error: no ff with a matching parameter list
double (*pf3)(int*) = ff; // error: return type of ff and pf3 don't match
```

**Function Pointer Parameters**

Just as with arrays (§ 6.2.4, p. 214), we cannot define parameters of function type
but can have a parameter that is a pointer to function. As with arrays, we can write a
parameter that looks like a function type, but it will be treated as a pointer:

```c++ 
// third parameter is a function type and is automatically treated as a pointer to function
void useBigger(const string &s1, const string &s2,
bool pf(const string &, const string &));
// equivalent declaration: explicitly define the parameter as a pointer to function
void useBigger(const string &s1, const string &s2,
bool (*pf)(const string &, const string &));
```



As we’ve just seen in the declaration of useBigger, writing function pointer types
quickly gets tedious. Type aliases (§ 2.5.1, p. 67), along with decltype (§ 2.5.3, p.
70), let us simplify code that uses function pointers:

```c++ 
// Func and Func2 have function type
typedef bool Func(const string&, const string&); // notice this typedef 
typedef decltype(lengthCompare) Func2; // equivalent type
// FuncP and FuncP2 have pointer to function type
typedef bool(*FuncP)(const string&, const string&); // notice this way of typedef 
typedef decltype(lengthCompare) *FuncP2; // equivalent type

// equivalent declarations of useBigger using type aliases
void useBigger(const string&, const string&, Func);
void useBigger(const string&, const string&, FuncP2);
```

**Returning a Pointer to Function **

```c++ 
using F = int(int*, int); // F is a function type, not a pointer
using PF = int(*)(int*, int); // PF is a pointer type
```

Here we used type alias declarations (§ 2.5.1, p. 68) to define F as a function type
and PF as a pointer to function type. The thing to keep in mind is that, unlike what
happens to parameters that have function type, the return type is not automatically
converted to a pointer type. We must explicitly specify that the return type is a pointer type:

```c++ 
PF f1(int); // ok: PF is a pointer to function; f1 returns a pointer to function
F f1(int); // error: F is a function type; f1 can't return a function
F *f1(int); // ok: explicitly specify that the return type is a pointer to function
```

declare f1 directly :

```c++ 
int (*f1(int))(int*, int);
```

