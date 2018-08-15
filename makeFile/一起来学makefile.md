## make的工作方式

1. 读入所有的Makefile
2. 读入被include的其他Makefile
3. 初始化文件的变量
4. 推到隐晦规则,并分析所有规则
5. 为所有的目标文件创建以来关系链
6. 根据以来关系,决定哪些目标要重新生成
7. 执行生成命令

> 1-5 步为第一个阶段，6-7 为第二个阶段。第一个阶段中，如果定义的变量被使用了，那么，
> make 会把其展开在使用的位置。但 make 并不会完全马上展开，make 使用的是拖延战术，如
> 果变量出现在依赖关系的规则中，那么仅当这条依赖被决定要使用了，变量才会在其内部展
> 开。



## 书写规则

### 语法

```
targets : prerequisites
command
...
或是这样：
targets : prerequisites ; command
command
...
```

> targets是文件名,以空格分开,可以使用通配符,可以试多个文件
>
> **command是命令行,如果在同一行,另一行开头要以tab键开始.如果一行太长了,可以用\ 来换行,这个有利于阅读**

> 判断是否是过时的,如果某个文件要比目标文件要新,那么目标就被认为是过时的,那么就需要重新生成

### 使用通配符

```
clean:
	rm -f *.o
```

> 这里直接删除所有的.o文件,我们一般会加一个-f,因为加上这个的话,就是强行执行,这样尽管文件不存在都不会报错

```
objects = *.o
```

> 这里变量也可以用通配符,但是并不意味着,*.o会进行展开,而是Objects的值就是"\*.o",其实就是简单的替换,相当于c中的宏,**简单替换**,
>
> 如果要展开:

```
objects := $(wildcard *.o)
```

### 文件搜索

#### 特殊变量VPATH

能够指定搜索的路径,比如

```
VPATH = src:../headers
```

> 上面指定了两个目录,并且按照当前目录,src,../headers顺序来寻找.
>
> 

#### 关键字vpath

可以指定不同文件到不同的文件目录里面搜索

三种用法

```
1、vpath <pattern> <directories>
为符合模式<pattern>的文件指定搜索目录<directories>。
2、vpath <pattern>
清除符合模式<pattern>的文件的搜索目录。
3、vpath
清除所有已被设置好了的文件搜索目录。
```

比如:

```
vpath %.h ../headers
```

> 该语句表示，要求 make 在“../headers”目录下搜索所有以“.h”结尾的文件。（如果
> 某文件在当前目录没有找到的话）

例如:

```
我们可以连续地使用 vpath 语句，以指定不同搜索策略。如果连续的 vpath 语句中出现
了相同的<pattern>，或是被重复了的<pattern>，那么，make 会按照 vpath 语句的先后顺
序来执行搜索。如：
vpath %.c foo
vpath % blish
vpath %.c bar
其表示“.c”结尾的文件，先在“foo”目录，然后是“blish”，最后是“bar”目录。
vpath %.c foo:bar
vpath % blish
而上面的语句则表示“.c”结尾的文件，先在“foo”目录，然后是“bar”目录，最后
才是“blish”目录。
```

### 伪目标

用特殊标记".PHONY"来标记一个目标为伪目标.

```makefile
.PHONY: clean
clean:
rm *.o temp
```



伪目标一般没有依赖的文件。但是，我们也可以为伪目标指定所依赖的文件。伪目标同样可以作为“默认目标”，只要将其放在第一个。一个示例就是，如果你的 Makefile 需要一口气生成若干个可执行文件，但你只想简单地敲一个 make 完事，并且，所有的目标文件都写在一个 Makefile 中，那么你可以使用“伪目标”这个特性：

```makefile
all : prog1 prog2 prog3
.PHONY : all
prog1 : prog1.o utils.o
cc -o prog1 prog1.o utils.o
prog2 : prog2.o
cc -o prog2 prog2.o
prog3 : prog3.o sort.o utils.o
cc -o prog3 prog3.o sort.o utils.o
```

```makefile
随便提一句，从上面的例子我们可以看出，目标也可以成为依赖。所以，伪目标同样也
可成为依赖。看下面的例子：
.PHONY: cleanall cleanobj cleandiff
cleanall : cleanobj cleandiff
rm program
cleanobj :
rm *.o
cleandiff :
rm *.diff		
```

## 书写命令

### 显示命令

在命令前加 "@"就不会输出命令,否则执行的命令全部都会输出例如:

![](https://ws1.sinaimg.cn/large/891f7782gy1fuadqe6j26j209f032dfo.jpg)

make参数-s则是全面禁止命令的显示

### 命令执行

```makefile
exec:
	cd /home; pwd
```

当使用make exec的时候就能使用默认的shell来执行这两条命令,

注意中间得有分号隔开,如果没有分号隔开只执行后面一条命令,甚至错误

### 命令出错

make会根据命令返回码来判断命令是否执行成功,如果执行失败,那么默认会终止所有规则的执行.

为了做到忽略命令的出错可以在makefile命令行前加上一个减号"-",标记为总认为是成功的,例如:

```makefile
clean:
	-rm -f *.o	
```

#### 全局方法

给make 加上-i,则会忽略全部的命令出错的判断

```makefile
make -i

```

--ignore-errors忽略参数意思

而如果一个规则是以“.IGNORE”作为目标的，那么这个规则中的所有命令将会忽略错误。

#### 继续执行

```makefile
make -k
```

或者 --keep-going,,这个参数的意思是，如果某规则中的命令出错了，那么就终目该规则的执行，但继续执行其它规则。



### 嵌套执行make

例如，我们有一个子目录叫 subdir，这个目录下有个 Makefile 文件，来指明了这个目
录下文件的编译规则。那么我们总控的 Makefile 可以这样书写：

```makefile
subsystem:
cd subdir && $(MAKE)
其等价于：
subsystem:
$(MAKE) -C subdir
```

总控makefile变量可以传递到夏季的Makefile中,但是不会覆盖下层的Makefile所定义的变量,除非指定了-e参数

```makefile
如果你要传递变量到下级 Makefile 中，那么你可以使用这样的声明：
export <variable ...>
如果你不想让某些变量传递到下级 Makefile 中，那么你可以这样声明：
unexport <variable ...>
```

```makefile
示例一：
export variable = value
其等价于：
variable = value
export variable
其等价于：
export variable := value
其等价于：
variable := value
export variable
示例二：
export variable += value
其等价于：
variable += value
export variable
```

## 变量

### 变量基础知识

变量在声明时就需要给予初值,使用时,就需要在变量名前加\$符号,但是最好使用小括号“（）”或是大括号“{}”把变量给包括起来。如果你要使用真实的“\$”字符，那么你需要用“$$”来表示。

```makefile
objects = program.o foo.o utils.o
program : $(objects)
cc -o program $(objects)
$(objects) : defs.h
```

**变量会在使用它的地方精确地展开，就像 C/C++中的宏一样**

### 用变量定义变量

#### 直接用=

```
foo = $(bar)
bar = $(ugh)
ugh = Huh?
all:
echo $(foo)
我们执行“make all”将会打出变量$(foo)的值是“Huh?”（ $(foo)的值是$(bar)，
$(bar)的值是$(ugh)，$(ugh)的值是“Huh?”）可见，变量是可以使用后面的变量来定义的。
```

这种方法可以用后面值来定义前面的值,这样小心出现递归死循环,应避免这样写



#### 用:=

相当于pascal的赋值符号.

```
x := foo
y := $(x) bar
x := later
其等价于：
y := foo bar
x := later
```



#### ?=

```
FOO ?= bar
```

> 其含义是，如果 FOO 没有被定义过，那么变量 FOO 的值就是“bar”，如果 FOO 先前被定义
> 过，那么这条语将什么也不做，其等价于：
> ifeq ($(origin FOO), undefined)
> FOO = bar
> endif

### 变量高级用法

#### “\$(var:a=b)”或是“\${var:a=b}”

把变量“var”中所有以“a”字串“结尾”的“a”替换成“b”字串。这里的“结尾”意思是“空格”或是“结束
符”。例如

```
foo := a.o b.o c.o
bar := $(foo:.o=.c)
```

> 这个示例中，我们先定义了一个“$(foo)”变量，而第二行的意思是把“$(foo)”中所
> 有以“.o”字串“结尾”全部替换成“.c”，所以我们的“$(bar)”的值就是“a.c b.c
> c.c”。

静态模式的替换

```
foo := a.o b.o c.o
bar := $(foo:%.o=%.c)
这依赖于被替换字串中的有相同的模式，模式中必须包含一个“%”字符，这个例子同
样让$(bar)变量的值为“a.c b.c c.c”。
```



#### 把变量的值再当成变量(嵌套定义)

```
first_second = Hello
a = first
b = second
all = $($a_$b)
```

这里的“\$a_\$b”组成了“first_second”，于是，$(all)的值就是“Hello”。

### 追加变量值

#### 使用+=

```
objects = main.o foo.o bar.o utils.o
objects += another.o
```

> 于是，我们的$(objects)值变成：“main.o foo.o bar.o utils.o another.o”（another.o
> 被追加进去了）

### override 指示符

可以覆盖命令行来的变量

```
override <variable> = <value>
override <variable> := <value>
当然，你还可以追加：
override <variable> += <more text>
```

### 多行变量

```
define 指示符后面跟的是变量的名字，而重起一行定义变量的值，定义是以 endef 关
键字结束。其工作方式和“=”操作符一样。变量的值可以包含函数、命令、文字，或是其
它变量。因为命令需要以[Tab]键开头，所以如果你用 define 定义的命令变量中没有以[Tab]
键开头，那么 make 就不会把其认为是命令。
下面的这个示例展示了 define 的用法：
define two-lines
echo foo
echo $(bar)
endef
```



### 目标变量

略

### 模式变量

略

## 使用条件判断

下面的例子，判断$(CC)变量是否“gcc”，如果是的话，则使用 GNU 函数编译目标。

```
libs_for_gcc = -lgnu
normal_libs =
foo: $(objects)
ifeq ($(CC),gcc)
$(CC) -o foo $(objects) $(libs_for_gcc)
else
$(CC) -o foo $(objects) $(normal_libs)
endif
```

```makefile
libs_for_gcc = -lgnu
normal_libs =
ifeq ($(CC),gcc)
libs=$(libs_for_gcc)
else
libs=$(normal_libs)
endif
foo: $(objects)
	$(CC) -o foo $(objects) $(libs)
```

一般写成下面形式

### 语法

```
条件表达式的语法为：
<conditional-directive>
<text-if-true>
endif
以及：
<conditional-directive>
<text-if-true>
else
<text-if-false>
endif
其中<conditional-directive>表示条件关键字
```

四个关键字

#### ifeq

```
ifeq (<arg1>, <arg2>)
ifeq '<arg1>' '<arg2>'
ifeq "<arg1>" "<arg2>"
ifeq "<arg1>" '<arg2>'
ifeq '<arg1>' "<arg2>"
```

判断相等

```
ifeq ($(strip $(foo)),)
<text-if-empty>
endif
这个示例中使用了“strip”函数，如果这个函数的返回值是空（Empty），那么
<text-if-empty>就生效。
```

#### ifneq

```
ifneq (<arg1>, <arg2>)
ifneq '<arg1>' '<arg2>'
ifneq "<arg1>" "<arg2>"
ifneq "<arg1>" '<arg2>'
ifneq '<arg1>' "<arg2>"
```

其比较参数“arg1”和“arg2”的值是否相同，如果不同，则为真。和“ifeq”类似。

#### ifdef

> ifdef <variable-name>
> 如果变量<variable-name>的值非空，那到表达式为真。否则，表达式为假。当然，
> <variable-name>同样可以是一个函数的返回值。注意，ifdef 只是测试一个变量是否有值，
> 其并不会把变量扩展到当前位置。

```
示例一：
bar =
foo = $(bar)
ifdef foo
frobozz = yes
else
frobozz = no
endif
示例二：
foo =
ifdef foo
frobozz = yes
else
frobozz = no
endif
第一个例子中，“$(frobozz)”值是“yes”，第二个则是“no”。
```



## 使用函数

### 语法

```
$(<function> <arguments>)
或是
${<function> <arguments>}
```

```
comma:= ,
empty:=
space:= $(empty) $(empty)
foo:= a b c
bar:= $(subst $(space),$(comma),$(foo))
```

> 注意这个空格变量的定义,不同与一般高级语言的双引号的定义

> 在这个示例中，\$(comma)的值是一个逗号。\$(space)使用了\$(empty)定义了一个空格，$(foo)
> 的值是“a b c”，$(bar)的定义用，调用了函数“subst”，这是一个替换函数，这个函数
> 有三个参数，第一个参数是被替换字串，第二个参数是替换字串，第三个参数是替换操作作
> 用的字串。这个函数也就是把$(foo)中的空格替换成逗号，所以$(bar)的值是“a,b,c”。

### 字符处理函数

#### subst

```
$(subst <from>,<to>,<text>)
名称：字符串替换函数——subst。
功能：把字串<text>中的<from>字符串替换成<to>。
返回：函数返回被替换过后的字符串。
```

```
$(subst ee,EE,feet on the street)，
把“feet on the street”中的“ee”替换成“EE”，返回结果是“fEEt on the
strEEt”。
```

#### patsubst

```
$(patsubst <pattern>,<replacement>,<text>)
名称：模式字符串替换函数——patsubst。
功能：查找<text>中的单词（单词以“空格”、“Tab”或“回车”“换行”分隔）是否符
合模式<pattern>，如果匹配的话，则以<replacement>替换。这里，<pattern>可以包括通
配符“%”，表示任意长度的字串。如果<replacement>中也包含“%”，那么，<replacement>
中的这个“%”将是<pattern>中的那个“%”所代表的字串。 （可以用“\”来转义，以“\%”
来表示真实含义的“%”字符）
返回：函数返回被替换过后的字符串。
```

```
$(patsubst %.c,%.o,x.c.c bar.c)
把字串“x.c.c bar.c”符合模式[%.c]的单词替换成[%.o]，返回结果是“x.c.o bar.o”
```

#### strip

```
$(strip <string>)
名称：去空格函数——strip。
功能：去掉<string>字串中开头和结尾的空字符。
返回：返回被去掉空格的字符串值。
```

```
示例：
$(strip a b c )
把字串“a b c ”去到开头和结尾的空格，结果是“a b c”。
```

#### findstring

```
$(findstring <find>,<in>)
名称：查找字符串函数——findstring。
功能：在字串<in>中查找<find>字串。
返回：如果找到，那么返回<find>，否则返回空字符串。
示例：
$(findstring a,a b c)
$(findstring a,b c)
第一个函数返回“a”字符串，第二个返回“”字符串（空字符串）
```

#### filter

```
$(filter <pattern...>,<text>)
名称：过滤函数——filter。
功能：以<pattern>模式过滤<text>字符串中的单词，保留符合模式<pattern>的单词。可以
有多个模式。
返回：返回符合模式<pattern>的字串。
示例：
sources := foo.c bar.c baz.s ugh.h
foo: $(sources)
cc $(filter %.c %.s,$(sources)) -o foo
$(filter %.c %.s,$(sources))返回的值是“foo.c bar.c baz.s”。
```

#### filter-out

```
$(filter-out <pattern...>,<text>)
名称：反过滤函数——filter-out。
功能：以<pattern>模式过滤<text>字符串中的单词，去除符合模式<pattern>的单词。可以
有多个模式。
返回：返回不符合模式<pattern>的字串。
示例：
objects=main1.o foo.o main2.o bar.o
mains=main1.o main2.o
$(filter-out $(mains),$(objects)) 返回值是“foo.o bar.o”。
```





## 隐含规则

### 自动化变量

```
$@
表示规则中的目标文件集。在模式规则中，如果有多个目标，那么，"$@"就是匹配于
目标中模式定义的集合。
$%
仅当目标是函数库文件中，表示规则中的目标成员名。例如，如果一个目标是"foo.a
(bar.o)"，那么，"$%"就是"bar.o"，"$@"就是"foo.a"。如果目标不是函数库文件（Unix
下是[.a]，Windows 下是[.lib]），那么，其值为空。
$<
依赖目标中的第一个目标名字。如果依赖目标是以模式（即"%"）定义的，那么"$<"将
是符合模式的一系列的文件集。注意，其是一个一个取出来的。
$?
所有比目标新的依赖目标的集合。以空格分隔。
$^
所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的，那个这个变量
会去除重复的依赖目标，只保留一份。
$+
这个变量很像"$^"，也是所有依赖目标的集合。只是它不去除重复的依赖目标。
$*
这个变量表示目标模式中"%"及其之前的部分。如果目标是"dir/a.foo.b"，并且目标的
模式是"a.%.b"，那么，"$*"的值就是"dir/a.foo"。这个变量对于构造有关联的文件名是比
较有较。如果目标中没有模式的定义，那么"$*"也就不能被推导出，但是，如果目标文件的
后缀是 make 所识别的，那么"$*"就是除了后缀的那一部分。例如：如果目标是"foo.c"，因
为".c"是 make 所能识别的后缀名，所以，"$*"的值就是"foo"。这个特性是 GNU make 的，
很有可能不兼容于其它版本的 make，所以，你应该尽量避免使用"$*"，除非是在隐含规则
或是静态模式中。如果目标中的后缀是 make 所不能识别的，那么"$*"就是空值。
当你希望只对更新过的依赖文件进行操作时，"$?"在显式规则中很有用，例如，假设有
一个函数库文件叫"lib"，其由其它几个 object 文件更新。那么把 object 文件打包的比较
有效率的 Makefile 规则是：
lib : foo.o bar.o lose.o win.o
ar r lib $?
在上述所列出来的自动量变量中。四个变量（$@、$<、$%、$*）在扩展时只会有一个文
件，而另三个的值是一个文件列表。这七个自动化变量还可以取得文件的目录名或是在当前
目录下的符合模式的文件名，只需要搭配上"D"或"F"字样。这是 GNU make 中老版本的特性，
在新版本中，我们使用函数"dir"或"notdir"就可以做到了。"D"的含义就是 Directory，就
是目录，"F"的含义就是 File，就是文件。
```





# 整体实例

![image.png](https://upload-images.jianshu.io/upload_images/6836439-eab08eed99e0d9be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)