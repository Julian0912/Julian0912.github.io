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





