[TOC]

### Constructors 

Unlike other member functions, constructors may not be declared as const (§
7.1.2, p. 258). When we create a const object of a class type, the object does not
assume its “constness” until after the constructor completes the object’s initialization. Thus, constructors can write to const objects during their construction.

**The Synthesized Default Constructor**

As we’ll, see the default constructor is special in various ways, one of which is that
if our class does not  explicitly define any constructors, the compiler will  implicitly
define the default constructor for us. The compiler-generated constructor is known as the synthesized default constructor. For most classes, this synthesized constructor initializes each data member of the class as follows:

**Defining the  Sales_data Constructors**

```c++ 
struct Sales_data {
    // constructors added
    Sales_data() = default;//we can ask the compiler to generate the constructor for us by writing = default after the parameter list.
    Sales_data(const std::string &s): bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):
    bookNo(s), units_sold(n), revenue(p*n) { }
    Sales_data(std::istream &);
    // other members as before
    std::string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

**Copy，Assignment, and destruction **

```c++ 
total = trans;
// it executes as if we had written
total.bookNo = trans.bookNo;
total.units_sold = trans.units_sold;
total.revenue = trans.revenue;
```

However, it is worth noting that many classes that need dynamic memory can (and generally should) use a vector or a string to manage the necessary storage.
Classes that use vectors and strings avoid the complexities involved in allocating and deallocating memory.

### Access Control and encapsulation 

In C++ we use access specifiers to enforce encapsulation:
• Members defined after a **public** specifier are accessible to all parts of the program. The public members define the interface to the class.
• Members defined after a **private** specifier are accessible to the member functions of the class but are not accessible to code that uses the class. The private sections encapsulate (i.e., hide) the implementation.

```c++ 
class Sales_data {
public: // access specifier added
Sales_data() = default;
Sales_data(const std::string &s, unsigned n, double p):
bookNo(s), units_sold(n), revenue(p*n) { }
Sales_data(const std::string &s): bookNo(s) { }
Sales_data(std::istream&);
std::string isbn() const { return bookNo; }
Sales_data &combine(const Sales_data&);
private: // access specifier added
double avg_price() const
{ return units_sold ? revenue/units_sold : 0; }
std::string bookNo;
unsigned units_sold = 0;
double revenue = 0.0;
};
```

**Using the class or struct Keyword**

>A class may define members before the first access specifier. Access to such members depends on how the class is defined. If we use the struct keyword, the members defined before the first access specifier are public; if we use class, then the members are private.
>
>**if there are zero access specifier , the members in class is public ,but the members in struct are private .**

**Friends**

A class can allow another class or function to access its nonpublic members by making that class or function a friend. A class makes a function its friend by including a declaration for that function preceded by the keyword friend:

```c++ 
class Sales_data {
// friend declarations for nonmember Sales_data operations added
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
// other members and access specifiers as before
public:
Sales_data() = default;
Sales_data(const std::string &s, unsigned n, double p):
bookNo(s), units_sold(n), revenue(p*n) { }
Sales_data(const std::string &s): bookNo(s) { }
Sales_data(std::istream&);
std::string isbn() const { return bookNo; }
Sales_data &combine(const Sales_data&);
private:
std::string bookNo;
unsigned units_sold = 0;
double revenue = 0.0;
};
// declarations for nonmember parts of the Sales_data interface
Sales_data add(const Sales_data&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
```

### 类的其他特性

可以自定义别名

```c++
class Screen {
public:
	typedef std::string::size_type pos;
private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
```

这样使用screen的用户就不知道screen使用了一个string对象来存放它的数据,因此通过吧pos定义成public成员可以隐藏screen实现的细节.

we can equivalLy use a type alias :

```c++ 
class Screen {
public:
// alternative way to declare a type member using a type alias
using pos = std::string::size_type;
// other members as before
};
```

**Member Functions of class  Screen**

add a constructor that will let users define the size and contents of the screen .

```c++ 
class Screen {
public:
    typedef std::string::size_type pos;
    Screen() = default; // needed because Screen has another constructor
    // cursor initialized to 0 by its in-class initializer
    Screen(pos ht, pos wd, char c): height(ht), width(wd),
    contents(ht * wd, c) { }
    char get() const // get the character at the cursor
    { return contents[cursor]; } // implicitly inline
    inline char get(pos ht, pos wd) const; // explicitly inline
    Screen &move(pos r, pos c); // can be made inline later
    private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
```

**Making Members  inline**

As we’ve seen, member functions defined inside the class are automatically inline .

```c++ 
inline // we can specify inline on the definition
Screen &Screen::move(pos r, pos c)
{
    pos row = r * width; // compute the row location
    cursor = row + c ; // move cursor to the column within that row
    return *this; // return this object as an lvalue
}
char Screen::get(pos r, pos c) const // declared as inline in the
class
{
    pos row = r * width; // compute row location
    return contents[row + c]; // return character at the given column
}
```

