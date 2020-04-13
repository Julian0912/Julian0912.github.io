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






