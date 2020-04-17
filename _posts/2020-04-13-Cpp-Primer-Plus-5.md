---
layout: post
title: 《C++ Primer Plus》第五版 笔记
categories: Programming
description: 
keywords: 
---

*这是《C++ Primer Plus》第五版的笔记。该书是我入门C++的第一本书。*

<!--more-->

## 第一章 预备知识

### 1.2 C++简史

#### 1.2.3 面向对象编程

与强调**算法**的过程性编程不同的是，OOP强调的是**数据**。OOP不像过程性编程那样，试图使问题满足语言的过程性方法，而是试图让语言来满足问题的要求。其理念是**设计与问题的本质特性相对应的数据格式**。

#### 1.2.4 C++和通用编程

通用编程（generic programming）是C++支持的另一种编程模式。它与OOP的目标相同，即使重用代码和抽象通用概念的技术更简单。不过OOP强调的是编程的数据方面，而通用编程强调的是**算法**方面。它们的侧重点不同。OOP是一个管理大型工程的工具，而通用编程提供了执行常见任务（如对数据排序或合并链表）的工具。

>   可能就是处理那些并非必须要定义类的问题？但也许没那么简单。

## 第二章 开始学习C++

### 2.1 进入C++

第一个程序

```c++
#include <iostream>

int main()
{
    using namespace std;
    cout << "Come up and C++ me some time"; //不换行
    cout << endl; //起到换行的作用
    cout << "You won't forget it!" << endl; //换行
    return 0;
}
```

注意，使用`cin`和`cout`的程序必须包含`iostream`文件。

#### 2.1.5 名称空间

名称空间在后面会详细介绍，简单说，它可以区分导入的不同文件中的相同名字的函数。

类、函数和变量是C++编译器的标准组件，它们都被放置在名称空间`std`中，当然是在头文件没有扩展名`.h`的情况下，如果头文件是`iostream.h`则不需要名称空间了。

>   关于头文件扩展名的省略是一堆从C到C++转换的历史故事，慢慢了解吧。

所以说，变量`cout`和`endl`都是定义在`iostream`中的。如果省略`using`编译指令，则代码可以这样写

```c++
std::cout << "Hello world" << std::endl;
```

或者当我们要特别使用名称空间中的某几个变量时，我们可以单独声明

```c++
using std::cout;
using std::endl;
```

这样，`std`名称空间中只有`cout`和`endl`对当前程序可用。

但通常来说，为了方便，我们直接把整个名称空间`std`都导进来，这样就不需要每次都加一个烦人的`std::`了。于是就产生了最开始的那一段代码。

当然如果我们要使用`iostream`内除了`std`的其它名称空间，还是要再单独导进来。

#### 2.1.6 使用cout进行C++输出

```c++
cout << "Hello world";
```

该语句表示把一个字符串发送给`cout`。`<<`表示了信息流动的路经。或者说，它把一个信息插入到了输出流中。

这里C++对操作符`<<`进行了**重载**。它既可以是按位左移操作符，也可以是插入操作符，具体含义依上下文确定。

`endl`是一个特殊的C++符号，它表示重起一行。诸如`endl`等对于`cout`来说有特殊含义的特殊符号被称为**控制符**。

也可以用以下方法换行

```c++
cout << "Hello world\n";
```

### 2.2 C++语句

观察如下代码

```c++
#include <iostream>

int main()
{
    using namespace std;
    
    int cts = 25;

    cout << "I have ";
    cout << cts;
    cout << " carrots.";
    cout << endl;
    cts--;
    cout << "Crunch! Crunch! Now I have " << cts << " carrots!" << endl;
    return 0;
}
```

所以`cout`可以直接输出变量（它足够聪明知道应该把整型转换为字符串输出），也可以接收由`<<`拼接的信息。

### 2.3 其它C++语句

观察如下代码

```c++
#include <iostream>

int main()
{
    using namespace std;
    
    int cts = 25;

    cout << "How many carrots do you have?" << endl;
    cin >> cts; //cin接收输入
    cout << "Here are two more." << endl;
    cts = cts + 2;
    cout << "Now you have " << cts << " carrots!" << endl;
    return 0;
}
```

#### 2.3.1 使用cin

语句`cin >> cts;`表明信息从`cin`流向`cts`。同样`cin`也足够聪明能够转换接收信息为可以接受的形式。

#### 2.3.2 使用cout进行拼接

如上所述，`cout`可以接受由`<<`拼接而成的多条信息，当信息很长时，也可以采用如下写法

```c++
cout << "Now you have "
    << cts
    << " carrots."
    << endl;
```

因为C++认为**换行符和空格是等同的、可替换的**。

### 2.4 函数

#### 2.4.3 用户定义的函数

C++的函数原型中，形参列表里可以只声明类型和数量，而不必声明形参名称。

C++也不允许函数中再定义函数。

#### 2.4.5 在多函数程序中使用using编译指令

使用名称空间（以`std`为例）的方式有很多种，以下几种的限制逐级加深

* 将`using namespace std;`放在函数定义之前，让文件中所有的函数都能够使用名称空间`std`中所有的元素。
* 将`using namespace std;`放在特定的函数定义中，让该函数能够使用名称空间`std`中的所有元素。
* 在特定的函数中使用类似`using std::cout;`这样的编译指令，而不是`using namespace std;`，让该函数只能使用指定的元素。
* 完全不使用编译指令`using`，在需要使用名称空间`std`中的元素时，使用前缀`std::`。

## 第三章 处理数据

### 3.1 简单变量

#### 3.1.6 整型常量

`cout`在输出数值时默认使用十进制，但也可以看情况修改。如下

```c++
#include <iostream>

int main()
{
    using namespace std;
    int h = 43;
    cout << hex;
    cout << "hex: " << h << endl;
    cout << dec;
    int d = 100;
    cout << "decimal: " << d << endl;
    return 0;
}
```

`hex`、`oct`和`dec`是`std`名称空间内的三个控制符，分别指示`cout`以十六进制、八进制和十进制格式显示整数。

注意，语句`cout << hex;`并不会打印什么东西，而且在一次调整后以后的输出格式都会遵循这次设置，若想重置则需要再次设置（如`cout << dec;`）。

#### 3.1.8 char类型：字符和小整数

`cout`有一个`put()`函数，该函数打印一个字符。如下

```c++
#include <iostream>

int main()
{
    using namespace std;
    char c = 'M';
    cout.put(c);
    return 0;
}
```

当然，像这种`cout.put(65);`也是可以的。

可以基于字符的八进制和十六进制编码来使用转义序列。例

```c++
#include <iostream>

int main()
{
    using namespace std;
    char a = '\n';
    char b = '\012';
    char c = '\xa';
    cout.put(a);
    cout.put(b);
    cout.put(c);
    return 0;
}
```

因为换行符的ASCII码为10，八进制为12，十六进制为a，所以以上`a`、`b`和`c`都打印换行符。

但是一般使用**符号转义序列**（即`\n`），因为可读性强，而且符号适用于任何编码方式。比如`\n`在任何编码方式里都表示换行符，但数字10可能只在ASCII码中才表示换行符（虽然事实可能并非如此，但可能会有其它符号符合这种情况）。

退格符的某种应用（挺有意思的）：

```c++
#include <iostream>

int main()
{
    using namespace std;
    cout << "Enter your code: ____\b\b\b\b";
    long code;
    cin >> code;
    cout << "Your have entered " << code << "...\n";
    return 0;
}
```

8位`char`可以表示基本字符集，另一种类型`wchar_t`（宽字符类型）可以表示扩展字符集。

`wchar_t`类型是一种整数类型，它有足够的空间，可以表示系统使用的最大扩展字符集。这种类型与另一种整型（底层类型）的长度
和符号属性相同。对底层类型的选择取决于实现，因此在一个系统中，它可能是`unsigned short`，而在另一个系统中，则可能是`int`。

`cin`和`cout`将输入和输出看作是`char`流，因此不适于用来处理`wchar_t`类型。`iostream`头文件的最新版本提供了作用相似的工具`wcin`和`wcout`，可用于处理`wchar_t`流。

另外，可以通过加上前缀`L`来指示宽字符常量和宽字符串。如

```c++
wchar_t wc = L'M';
wcout << wc << endl;
```

宽字符集在使用Unicode或ISO 10646时会比较有用。

### 3.2 const限定符

C++中可以使用`const`来修饰常量，并且比`#define`要好，因为它可以限定常量的作用域和类型。

```c++
#include <iostream>

int main()
{
    using namespace std;
    const int MONTHS = 12;
    cout << MONTHS << endl;
    return 0;
}
```

>   但会不会出现C中那种可以通过指针修改常量而造成常量不安全的问题呢？

### 3.3 浮点数

计算机将带小数的数字分成两部分存储。一部分表示值，另一部分用于对值进行放大或缩小。

打个比方，对于数字34.1245和34124.5，它们除了小数点的位置不同外，其他都是相同的。可以把第一个数表示为0.341245（基准值）和
100（缩放因子），而将第二个数表示为0.341245（基准值相同）和10000（缩放因子更大）。缩放因子的作用是移动小数点的位置，术语浮点因此而得名。

对于C++来说，数字存储基于二进制，因此缩放因子是2的幂。

### 3.4 C++算术操作符

如果`/`操作符两边的操作数都是整数，则商也为整数，即丢弃掉小数部分。

`%`操作符两边必须是整数，不能是浮点数。（python中可以是浮点数，其求模算法同时支持正负和小数）

对于`float`，C++只保证6位有效位。有效位不是指小数位，而是指有意义的六位数。比如61.2400有4个有效位，12000.0只有两个有效位，但40342.242保证6个有效位后四舍五入为40342.2。

#### 3.4.4 类型转换

C++中的强制类型转换有三种，如下

```c++
int n = 100;
long g1 = (long)n;
long g2 = long(n);
long g3 = static_cast<long>(n);
```

第一种来源于C，第二种是纯粹的C++风格。第三种是非常严格的一种强制类型转换，后面会讲。

## 第四章 复合类型

### 4.1 数组

注意以下数组初始化的区别

```c++
int arr1[100]; //未初始化，元素值不明
int arr2[100] = {1}; //第一个元素初始化为1，其它元素为0
int arr3[100] = {0}; //所有元素都被初始化为0
```

### 4.2 字符串

#### 4.2.1 拼接字符串常量

以下三个字符串的输出相同

```c++
cout << "hello" "world\n"; //程序会忽略两个字符串间的空格和换行
cout << "hello"
    "world\n";
cout << "helloworld\n";
```

#### 4.2.3 字符串输入

`cin`使用空白（空格、制表符和换行符）来定字符串的界。所以`cin`在从键盘获取字符串时只读取一个单词。

#### 4.2.4 每次读取一行字符串输入

`cin`对象下的成员函数`getline()`可以读取一行字符串。它以**换行符**标记结束，因此可以读入空格。但它读入时会**把换行符舍弃掉**，并以`\0`代替之（即标记字符串结束的0）。例

```c++
#include <iostream>

int main()
{
    using namespace std;
    char name[20];
    char name2[20];
    cout << "first name:\n";
    cin.getline(name, 20);
    cout << "second name:\n";
    cin.getline(name2, 20);
    cout << name << " " << name2 << endl;
    return 0;
}
```

`getline()`接收两个参数，第一个是存储字符串的变量，第二个是读取字符的数量。该函数要么遇到换行符结束读取，要么达到最大字符数停止读取。假设第二个参数为20，则函数只读取前19个字符，最后一个设为`\0`。

另一个类似的函数为`get()`，与`getline()`稍不同的是，它不会把换行符舍弃掉，而是让它继续**留在输入队列里**。因此，如果单纯把上一段代码中的`getline`换成`get`，程序将不能正确读取第二次输入。因为`get()`也是把换行符当作结束标记，而在第一次输入时因为把换行符留在了输入队列里，所以第二次输入时看到的第一个字符就是换行符，然后直接结束输入了。

解决办法就是用一个没有参数的`cin.get()`函数，它是`get()`的一个变体（实际上它有很多变体，表现为参数不同），可以接收换行符，即把换行符从输入队列里拿掉，所以代码如下，可以正常使用

```c++
#include <iostream>

int main()
{
    using namespace std;
    char name[20];
    char name2[20];
    cout << "first name:\n";
    cin.get(name, 20);
    cin.get(); //接收了一个换行符
    cout << "second name:\n";
    cin.get(name2, 20);
    cout << name << " " << name2 << endl;
    return 0;
}
```

值得注意的一点是，`cin.get()`本身会返回一个`cin`对象，因此它又可以调用`get()`，所以上段代码的第一次输入可以写为

```c++
cin.get(name, 20).get();
```

这样就省去了单独写一个`cin.get();`。同样，`cin.getline()`也可以这样操作。

乍一看貌似`cin.getline()`要比`cin.get()`简单多，但实际上，后者因为能接收换行符，所以可以判断字符串是因为遇到换行符所以停止读入的，还是因为字符串超出容量所以切断读入的。

#### 4.2.5 混合输入字符串和数字

观察如下代码

```c++
#include <iostream>

int main()
{
    using namespace std;
    cout << "What year?\n";
    int year;
    cin >> year;
    cout << "What name?\n";
    char name[20];
    cin.getline(name, 20);
    cout << "The year is " << year << ", and the name is " << name << endl;
    return 0;
}
```

依然没有机会进行第二次输入，因为`cin >> year;`也会把换行符留在输入队列里，这样第二次输入时刚开始就结束了。

解决办法可以是把该语句换为`(cin >> year).get();`，因为`cin >> year`也返回一个`cin`对象。

但是C++里一般用指针操作字符串，而非数组。

>   感觉这里白讲这么多了……

### 4.3 string类简介

`string`类**位于名称空间`std`中**。使用时**需要包含头文件`string`**。

```c++
#include <iostream>
#include <string>

int main()
{
    using namespace std;
    string str = "hello world";
    cout << str << endl;
    cout << "Your name?\n";
    string name;
    cin >> name;
    cout << "Hello, " << name << endl;
    cout << "The first letter of your name is " << name[0] << endl;
    return 0;
}
```

#### 4.3.1 赋值、拼接和附加

`string`对象可以使用`+`或`+=`进行字符串的拼接，但字符数组不能。

#### 4.3.2 string类的其它操作

求字符串长度

```c++
string str = "hello";
cout << str.size() << endl;
```

在C中使用`strlen()`函数来求长度，该函数在C++的`cstring`头文件里，接收目标字符串为参数。

#### 4.3.3 string类I/O

`string`对象在**初始化前长度为0**。在使用`cin`读取时依然会遇到空白就结束，因此在读取一行时，要这样处理

```c++
#include <iostream>
#include <string>

int main()
{
    using namespace std;
    string name;
    getline(cin, name);
    cout << "Hello, " << name << ".\n";
    return 0;
}
```

`getline()`函数包含在头文件`string`中，在名称空间`std`中。它接收`cin`和一个字符串变量为参数，不需要指定长度了。

### 4.4 结构简介

```c++
#include <iostream>
#include <string>

int main()
{
    using namespace std;
    struct person
    {
        string name;
        int age;
        double wage;
    };
    person stu = { "Jack", 19, 100.0 };
    person tea =
    {
        "Karl",
        23,
        3000.0
    };
    cout << stu.name << " is " << stu.age << " years old.\n";
    cout << tea.name << " owns " << tea.wage << " per week.\n";
    return 0;
}
```

结构的使用与C基本无异，但是在C++中，结构变量的声明可以省略关键字`struct`。

但C++建议把结构放在全局的位置，而非在`main`函数内，若如此，注意要使用`string`对象需要引入名称空间`std`。

#### 4.4.4 结构数组

```c++
#include <iostream>
#include <string>

int main()
{
    using namespace std;
    struct person
    {
        string name;
        int age;
        double wage;
    };
    person students[2] =
    {
        {"Jack", 19, 3000.0},
        {"Karl", 20, 2000.0}
    };
    cout << students[0].name << " is " << students[0].age << " years old.\n";
    cout << students[1].name << " owns " << students[1].wage << " per week.\n";
    return 0;
}
```

### 4.5 共用体

就是联合，翻译不同……

可以这样理解，一个联合内有多种不同的变量类型，一个联合变量可以多次赋值，每次可以赋值为不同的类型，但只能存储一种类型，因此联合的大小是所有类型中最大的那个。

```c++
#include <iostream>

int main()
{
    using namespace std;
    union one4all
    {
        int i_var;
        float f_var;
        char c_var[20];
    };
    one4all var;
    var.i_var = 15;
    cout << var.i_var << endl;
    var.f_var = 16.5f;
    cout << var.f_var;
    return 0;
}
```

假设有批商品id中有些是整型，有些是字符串，则可以设置一个整型和字符串的联合，来存储这批商品id。

### 4.6 枚举

```c++
#include <iostream>

int main()
{
    using namespace std;
    enum colors { red, orange, yellow, green, teal, blue, purple };
    colors book = orange;
    enum bits { one = 1, two = 2, four = 4, eight = 8 };
    enum vars {zero, null, none = 0, first};
    vars v = null;
    cout << v; //实际输出1
    return 0;
}
```

严格来说，枚举类型没有算术操作，枚举变量一般用于`switch`。

#### 4.6.2 枚举的取值范围

上限：找到枚举量的最大值，找到大于这个最大值的最小的2的幂，减去1，就是这个枚举的取值上限。例最大值为101，则比它大的最小的2的幂是128，因此该枚举的取值上限是127。

下限：找到枚举量的最小值，如果该最小值大于0，则取值下限为0，如果小于0，则找小于这个最小值的最大的2的幂，加上1，就是这个枚举的取值上限。例最小值-6，则比它小的最大的2的幂是-8，因此该枚举的取值下限为-7。

在C++中可以通过**强制转换**把不属于枚举值但在枚举取值范围内的**整型**赋值给枚举变量。

```c++
enum bits { one = 1, two = 2, four = 4, eight = 8 };
bits var = bits(6);
```

### 4.7 指针和自由存储空间

#### 4.7.2 指针的危险

一定要在对指针应用解除引用操作符（*）之前，将指针初始化为一个确定的、适当的地址。

#### 4.7.5 使用delete来释放内存

```c++
#include <iostream>

int main() {
    using namespace std;
    int *p = new int;
    *p = 10;
    cout << "The address is " << p << ", the value is " << *p << endl;
    delete p; //该语句并不会删除指针p，而是释放了p的内存，这样程序再次申请内存时还可以找到这块内存
    *p = 20;
    cout << "Second life: \n";
    cout << "The address is " << p << ", the value is " << *p << endl;
    return 0;
}
```

注意，**`delete`只能释放由`new`申请的内存**。而且，对于不再使用的内存块，最好**释放它**，防止内存溢出。

因此，`new`和`delete`总是成对出现。

#### 4.7.6 使用new来创建动态数组

对于一些小数据，可以直接创建变量，此时内存会被分配，不管这个变量用到了没有。

如果使用`new`创建，则只在需要的时候才会占用内存，因此适合大数据。

对于用`new`创建的数组，称为动态数组

```c++
int *p = new int[10];
```

指针`p`获得数组的第一个元素地址。并且在运行时才会确定大小。（普通数组在编译时就会确定大小）

也就是说，这个10可以是一个其它**变量**，其值随运行结果而定。

注意，使用完后要释放内存，**`new`和`delete`总是成对出现的**。

```c++
delete[]p; //删除数组要加方括号，删除普通指针不用
```

观察下段代码，体会使用指针操作数组与使用数组名操作数组的不同。

```c++
#include <iostream>

int main() {
    using namespace std;
    int *p = new int[10];
    p[0] = 3;
    p[1] = 5;
    p[2] = 8;
    cout << "p[0] is " << p[0] << endl;
    p = p + 1;
    cout << "Now p[0] is " << p[0] << ", and p[1] is " << p[1] << endl;
    p = p - 1; //将指针移回起点，确保delete能正确释放内存
    delete[]p;
    return 0;
}
```

### 4.8 指针、数组和指针算术

#### 4.8.2 指针和字符串

C风格的字符串处理

```c++
#include <iostream>
#include <cstring>

using namespace std;

char *get_name();

int main() {
    char *name;
    name = get_name();
    cout << name << " at " << (int *) name << endl;
    delete name; //一般不把new和delete分开，但分开也可以，只是不推荐
    name = get_name();
    cout << name << " at " << (int *) name << endl;
    delete name;
    return 0;
}

char *get_name() {
    char temp[80];
    cout << "Enter your name:";
    cin >> temp;
    char *pn = new char[strlen(temp) + 1]; //不可以直接char *pn;
    strcpy(pn, temp);
    return pn;
}
```

要注意的问题很多，要解释起来也很麻烦，因此不如用C++的`string`库。（上段代码**可能**有诸多问题）

#### 4.8.3 使用new创建动态结构

在用指针操作结构时，要用到`->`操作符，如果用结构名操作结构，则用`.`操作符。

```c++
#include <iostream>

int main() {
    using namespace std;
    struct person {
        char name[20];
        int age;
        double wage;
    };
    person *p = new person;
    cout << "Enter the name:";
    cin >> (*p).name; //(*p)相当于结构变量名
    cout << "Enter the age:";
    cin >> (*p).age;
    cout << "Enter the wage:";
    cin >> (*p).wage;
    cout << "Name: " << p->name << endl //指针操作
        << "Age: " << p->age << endl
        << "Wage: " << p->wage << endl;
    delete p; //释放内存
    return 0;
}
```

#### 4.8.4 自动存储、静态存储和动态存储

自动存储指在特定代码块内活动的变量，它们在代码块运行结束后会自动释放空间。

静态存储指在整个程序运行周期内都存在的存储方式。比如静态变量。

动态存储有别于前两种，它们在内存的一片自由区域（堆），由`new`分配和`delete`释放。

如果不释放将可能导致内存泄漏，将十分严重，用完了**一定要释放**！

## 第五章 循环和关系表达式





