---
layout: post
title: 《C语言大全第4版》笔记（第一部分）
categories: Programming
description: 
keywords: 
---

*前期的C语言自学笔记主要依据黑马程序员B站视频，因视频学习时间过长，故转战书本学习。章节之间不连续的原因是已熟知的章节没有做笔记。*

<!--more-->

## 第二章 C表达式

### 2.5 类型修饰符

#### 2.5.1 const

在形参前加`const`可以防止函数内部对原变量的修改。通常如果不希望函数修改实际参数，则会在形式参数前加`const`。

```c
#include <stdio.h>

void func(const char* s)
{
    //s[5] = 'I'; //会报错，但如果参数不加const，就能正常修改，输出"this Is a test"
}

int main(void)
{
    char str[15] = "this is a test";
    func(str);
    printf("%s\n", str);
    return 0;
}
```

如果使用指针修改，虽然目标字符可以修改成功，但修改部分以后的字符就无法输出了。

```c
#include <stdio.h>

void func(const char* s)
{
    int* p = &s[5];
    *p = 'I';
}

int main(void)
{
    char str[15] = "this is a test";
    func(str);
    printf("%s\n", str);
    return 0;
}
```

>   为什么呢？

#### 2.5.2 volatile

如果变量不出现在赋值号左边，则程序认为变量没有更改，则会优化后续某些用到该变量的表达式，比如直接用值替换掉，但有些变量是随硬件信息变化的（如时钟值），虽然程序没有修改，但其值也会变化。在此种变量前加`volatile`修饰符可以防止程序优化此类变量。

### 2.6 存储类型说明符

C有四种存储类型说明符，`extern`，`static`，`register`，`auto`。

#### 2.6.1 extern

C语言中有三种链接，**外部链接**，**内部链接**，**无链接**。通常函数和全局变量有外部链接，它们在程序的所有文件中都是可以使用的；用`static`修饰的变量有内部链接，只能在声明它的文件内使用；局部变量无链接，只在自己的块域内使用。

```c
#include <stdio.h>

int main(void)
{
    //如果此处不加extern，则定义了变量，却没有赋值，后续会报错
    //添加extern后，说明只声明变量，在别的地方定义，就不会报错
    extern int a, b;
    printf("a: %d, b: %d\n", a, b);
    return 0;
}
//此处只是因为定义放在了main函数之后，才需要加extern
int a = 10, b = 20;
```

**变量可以有很多声明，但只能有一个定义。**

`extern`主要用于多文件编译。比如一个文件使用的变量是在另一个文件里定义的，则在这个文件里就可以用`extern`声明一下变量，让程序去别的文件里找定义。

#### 2.6.2 static变量

```c
#include <stdio.h>

int series(void)
{
    //静态局部变量，能在多次调用这个函数时保持上一次修改的新值
    //本质是创建一个永久存储空间，不会在函数结束时毁灭
    //但仍然是局部变量，只在函数内可见
    static int num = 10;
    num = num + 10;
    return num;
}

int main(void)
{
    printf("%d\n", series());
    printf("%d\n", series());
    printf("%d\n", series());
    return 0;
}
```

对于静态全局变量，它只能对声明它的文件可见，因此不能从别的文件导入，省去了很多副作用。

#### 2.6.3 register变量

`register`将修饰的变量存在寄存器中而非内存中，所以加快了访问该变量的速度。但代码中可以得到寄存器优化的变量是有限的，超限的寄存器变量将会变为普通变量。

`register`只能用于**局部变量**和**函数形参**，全局寄存器变量是非法的。

虽然`register`已经得到扩展，但实际上它还是只对**整型**和**字符型**有实际作用。

用`register`修饰的变量不能用`&`寻址，因为寄存器无法编址。

### 2.9 操作符

#### 2.9.12 逗号(,)操作符

逗号连接一串表达式，逗号左边求值为`void`型，一串表达式的最右侧为整个表达式的值。

```c
#include <stdio.h>

int main(void)
{
    int x, y; //只能先定义
    int a = (x = 4, y = 7, x + y); //必须加括号，因为逗号的优先级比赋值号低
    printf("%d\n", a); //输出11
    return 0;
}
```

## 第三章 语句

### 3.3 重复(Iteration)语句

#### 3.3.1 for循环

for语句的一般形式：

```
for (initialization; condition; increment) statement;
```

一般来说，`initialization`是赋值语句，`condition`是条件表达式，`increment`定义每次循环后如何修改变量。理论上三者都是**可选的**。

程序的执行顺序：

```
initialization;
condition;
statement;
increment;
condition;
statement;
increment;
condition;
……
```

#### 3.3.3 无限循环

当条件表达式不存在时，则认为它是正确的。

```c
#include <stdio.h>

int main(void)
{
    for (;;) printf("run forever\n");
    return 0;
}
```

#### 3.3.5 在for循环中声明变量

以下例子**在C89中不合法**，在C99中合法。但该写法已非常常见。

```c
for (int i = 0; i < 5; i++) printf("%d\n", i);
```

C89应在循环初始化外声明所需变量。

```c
int i;
for (i = 0; i < 5; i++) printf("%d\n", i);
```

## 第四章 数组和串

### 4.3 向函数传一维数组

在接受数组的函数中，定义数组形参的方法有三种：**指针**，**定尺寸数组**，**无尺寸数组**。以下效果等同：

```c
void func1(int* x);
void func2(int x[10]);
void func3(int x[]);
```

三种声明都是在告诉函数要接受一个整型数组了。但C语言中并不能把一整个数组传入函数，所以它们**实际接收的都是一个指针**。且因为C语言不进行边界检查，所以参数有没有尺寸也无所谓，并不会引起内存分配。

### 4.5 二维数组

在向函数传递二维数组时，形参的定义中，第一维可以没有大小，但第二维必须要有，如下：

```c
void func(int x[][10]);
```

因为要告诉程序每一行有多长，程序才能正确计算二维坐标。（多维数组同理）

### 4.7 指针的下标操作

对于数组`char p[10]`，`p`等同于`&p[0]`，而专业的C程序中不会出现后者。

为了正确运算指针算术（后面几章会有），将数组指针转换为基类型指针的算法是必不可少的。有时，程序中通过指针运算操作数组，原因在于**指针运算一般快于数组下标**。

>   既然`p`等同于`&p[0]`，那为什么要转换数组指针为基类型指针呢？（`(int*)&p[0]`）
>
>   例如，对于二维数组`int a[10][10]`，要取`a[1][2]`，还可以`*((int*)a+12)`。既然`a`本身就是一个指针，又为什么要强制转换为`int*`呢？

### 4.8 数组初始化

C99允许非常量初始化字符用于本地数组，而C89要求常量初始化字符用于所有数组。

>   啥叫本地数组？
>
>   **答：本地数组是指带有块域或原型域的数组。**
>
>   自我理解：大概就是不能是全局数组？

```c
void func(int dim)
{
    char str[dim];
    /* statements */
}
```

## 第五章 指针

### 5.4 指针表达式

#### 5.4.2 指针转换

指针操作是相对指针的基类型而执行的。虽然技术上指针可以指向对象的其它类型，但它始终认为自己指向的是其基类型的对象。因此强制指针转换可能会出现各种问题。

```c
#include <stdio.h>

int main(void)
{
    double x = 100.1, y;
    int* p;
    p = (int*)&x;
    y = *p;
    printf("%f\n", y); //并不能得到正确的结果
    return 0;
}
```

虽然在C语言中由其它类型指针转到`void*`类型或者反过来都不会有问题，但C++中非强制转换的各种转换都是非法的，包括`void*`，所以一般来说在C程序中也舍弃了所有的指针转换。

**务必确保指针操作的对象与基类型兼容。**

#### 5.4.3 指针算术操作

可以施加于指针的算术操作只有**加法**和**减法**。

实际上指针可以参与的算术屈指可数，只有**增量**，**减量**，**指针加减整数**，**指针相减**。

### 5.5 指针和数组

#### 5.5.1 指针数组

指针数组的声明方式、赋值、取值：

```c
int* x[10];
x[2] = &var;
v = *x[2];
```

向函数传指针数组的参数：

```c
void display_array(int* q[])
{
    int t;
    for (t = 0; t < 10; t++)
        printf("%d\n", *q[t]);
}
```

注意，上述函数形参不能写成`int* q`。

指针数组常用于放置指向串的指针，例如：

```c
void syntax_error(int num)
{
    static char* err[] = {
        "Cannot open file.\n",
        "Read Error.\n",
        "Write Error.\n",
        "Media Failure.\n"
    }
    printf("%s", err[num]);
}
```

### 5.7 指针初始化

在python中，创建和使用一个字符串列表很容易也很方便，但C语言中并没有特别提供字符串类型，所以一般的字符串数组像这样声明：

```c
char* str[] = {"hello", "world", NULL};
```

没错这就是一个指针数组，如上节所言。

但通常会在串数组最后加一个常量`NULL`（该常量定义在`stdio.h`头文件中），用来告诉程序数组到这里结束了。

遍历时如下：

```c
for (int i = 0; str[i]; i++) printf("%s\n", str[i]);
```

如果数组最后没有`NULL`，可能会发生一些不了解的问题，例如不能退出遍历等。

同样的，对于创建字符串，通常用这些方式：

```c
char s[] = "hello";
char* t = "world";
```

可以看出，`s`和`t`实际都是指针，而指针是不能存储一整个字符串的。那个字符串实际被存到哪了呢？

C编译程序创建了一种叫“串表”的东西，用来存储字符串，然后把串表的地址传给指针。这样就可以用指针操作字符串了。

```c
printf("%s, %s", s, t);
```

### 5.8 函数指针

函数名也是函数的地址，跟数组名是数组地址相同。所以当把一个指针指向一个函数地址时，就可以通过指针调用函数。

函数指针的声明方式：

```c
int (*p)(const char*, const char*);
```

这个声明表示这个函数指针名字叫`p`，有两个参数，都是`const char*`，返回值是`int`类型。参数，名字和返回值都可变，但声明方式固定。

注意，`(*p)`中的**括号一定不能省略**。

在调用时，假设两个参数为`a` `b`，则有两种方式：

```c
(*p)(a, b);
p(a, b);
```

通常**使用第一种方式**，因为读代码的人在看到第一种调用时可以明确知道`p`是一个函数指针而非函数名。

函数指针常用于函数传参。这样不同的函数名可以传入同一函数内用一条语句统一调用。

### 5.9 动态分配函数

动态分配是程序在运行中获取内存的方法。全局变量在编译时分配，非静态局部变量使用栈空间，两者都不能在运行时增减。但有些程序可能在运行时需要大小可变的空间，比如动态数据结构链表或者二叉树等。

动态分配函数从堆中获取内存。堆是一个系统的自由内存区，一般很大。

动态分配系统的核心函数只有`malloc()`和`free()`。两者都包含在头文件`stdlib.h`中。

`malloc()`的原型是

```c
void* malloc(size_t number_of_bytes);
```

由此看出，该函数返回一个`void*`类型。该类型在C语言中可以非强制转换成其它指针类型，所以可以这样使用

```c
char* p;
p = malloc(1000);
```

`malloc(1000)`返回了一个`void*`类型，赋给了`char*`类型的变量，并不会报错。但在C++中是不行的，需要加强制转换。而且，为了让C程序与C++兼容，**现在也会在C程序里加强制转换**。所以上例：

```c
char* p;
p = (char*)malloc(1000);
```

由于有些数据类型，比如`int`会由于系统不同而占据不同的空间大小，所以通常申请内存会这个样子处理

```c
p = malloc(50*sizeof(int));
```

这样申请了50个整数的空间大小，还保证了可移植性。

虽然堆很大，但也是有限的，所以最好在申请内存后检查一下是否真的分配到了空间。如果分配到了，`malloc()`会返回空间的首地址，如果没有，则会返回`NULL`。所以可以这样检查

```c
p = malloc(100);
if (!p)
{
    printf("Out of memory!\n");
    exit(1);
}
```

`exit(1)`处可以用其它错误处理函数替代，总之就是要防止使用空指针。

`free()`是`malloc()`的逆函数，它将后者申请到的内存再释放回去。原型是

```c
void free(void* p);
```

`p`指向原先`malloc()`分配到的内存指针。

注意，**绝不能用无效指针调用`free()`**。

#### 5.9.1 动态分配的数组

```c
int (*p)[10];
```

>   没搞明白这个定义到底是什么意思，后面再来补坑吧。

### 5.10 由restrict修饰的指针

`restrict`是C99标准新加的仅适用于指针的类型修饰符，将在后面详细介绍。

### 5.11 与指针有关的问题

指针错误的性质特别严重，且通常难以排查，应全力防止。以下研究几种指针错误的例子。

**使用未初始化的指针**：

```c
int main(void)
{
    int x, * p;
    x = 10;
    *p = x; //err
    return 0;
}
```

未初始化指针，程序会给指针随机分配地址，在地址充足的情况下，通常会分配到安全地址，但随着程序和数据越来越多，难免分配到不安全的地址，如系统地址。所以要严格避免这种情况。

**错误理解指针**：

```c
#include <stdio.h>

int main(void)
{
    int x, * p;
    x = 10;
    p = x;
    printf("%d", *p);
    return 0;
}
```

此程序相当于把地址编号10赋给了`p`，而这个地址是未知的，通常输出不确定。`p = x`应为`p = &x`。

**认为相邻数组顺序排列**：

```c
int main(void)
{
    int first[5], second[5];
    int * p, t;
    p = first;
    for (t = 0; t < 10; t++) *p++ = t; //err
    return 0;
}
```

实际上两个数组不一定相邻，也不一定`first`总在`second`前面，所以不能在同一个循环里赋值。

**一个严重的BUG**：

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[80];
    char* p;
    p = s;
    do
    {
        gets_s(s, 80);
        while (*p) printf("%d\n", *p++);
    } while (strcmp(s, "done"));
    return 0;
}
```

此段程序本意在于每次获取一段字符串就输出，但错误在于`p = s`在循环外，这说明它只被设置一次，而在循环内，在输出字符串后，指针`p`的地址已经到了字符串的末尾而未得到重置，可能就会超出串的长度限制，将数据覆盖到其它变量或其它程序。这是个简单却严重的循环错误，解决办法就是把`p = s`放到`do`循环内第一行。

## 第六章 函数

### 6.3 函数的变元

变元就暂时理解为跟函数形参相对的实际参数吧。

#### 6.3.1 值调用和引用调用

像函数形参传递值时有两种方法，值调用传递的是**变元的拷贝**，函数内修改形参不会改变变元；引用调用传递的是**变元地址**，在函数内通过地址访问对象，对对象的修改也会改变变元。

一般来说，C使用值调用向函数传递变元，这样函数内就不能修改实参。

```c
#include <stdio.h>

int sqr(int x)
{
    x = x * x;
    return x;
}

int main(void)
{
    int t = 10;
    printf("%d %d", sqr(t), t); //t还是10
    return 0;
}
```

#### 6.3.2 引用调用

下面是一个引用调用的例子：

```c
#include <stdio.h>

void swap(int* x, int* y)
{
    int temp;
    temp = *x;
    *x = *y;
    *y = temp;
}

int main(void)
{
    int i = 10, j = 20;
    printf("before: %d %d\n", i, j);
    swap(&i, &j); //注意传递的是地址
    printf("after: %d %d\n", i, j);
    return 0;
}
```

#### 6.3.3 用数组调用

向函数传递数组时，实际传递的是数组的地址，所以这是一个标准值调用的例外。这种情况下，函数内对数组的修改实际是修改了真正的数组（这跟python列表作函数参数时相似），观察下例

```c
#include <stdio.h>
#include <ctype.h>

void print_upper(char* s)
{
    for (int i = 0; s[i]; i++)
    {
        s[i] = toupper(s[i]);
        putchar(s[i]);
    }
    printf("\n");
}


int main(void)
{
    char str[80];
    gets_s(str, 80);
    print_upper(str);
    printf("%s\n", str); //实际数组也被修改了
    return 0;
}
```

如果不想修改实际的数组，可以把`print_upper()`中`for`循环内的语句换成`putchar(toupper(s[i]))`。

### 6.4 main()的变元argc和argv

有时需要向程序传入信息。我们一般通过命令行变元向主函数`main()`传递信息。命令行变元是操作系统命令行中执行程序名字之后的信息。

有两个内设参量用于接受命令行变元，一个是`argc`，另一个是`argv`。`argc`是整型变量，存放命令行中变元的总数，因为程序名也算一个，所以`argc`的值最小为1；`argv`是指针，指向由字符串指针组成的数组（即字符串数组），数组中每个元素指向一个命令行变元，所有命令行变元都是字符串。观察下例

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char* argv[])
{
    if (argc != 2)
    {
        printf("You have to type your name!\n");
        exit(1);
    }
    printf("Hello %s\n", argv[1]); //第一个命令行变元(即argv[0])总是程序的名字
    return 0;
}
```

**注意`argv`的声明方式**。假设文件名叫`temp.c`，在命令行中输入如下语句

```
PC > gcc temp.c -o temp.exe
PC > ./temp.exe Julian
Hello Julian
```

注意，命令行变元之间必须用**空格**或**制表符**分隔，逗号和分号都不是分隔符。

### 6.5 返回语句

#### 6.5.3 返回指针

返回指针的函数必须明确声明返回的指针类型。

```c
#include <stdio.h>

char* match(char c, char* s)
{
    while (c != *s && *s) s++;
    return s; //返回一个地址或者NULL
}

int main(void)
{
    char s[10], * p, ch;
    gets_s(s, 10);
    ch = getchar();
    p = match(ch, s);
    if (p) printf("%s\n", p);
    else printf("Not found\n");
    return 0;
}
```

### 6.7 递归

```c
#include <stdio.h>

int factr(int n)
{
    if (n == 1) return 1;
    int answer = factr(n - 1) * n;
    return answer;
}

int main(void)
{
    printf("%d\n", factr(10));
    return 0;
}
```

C语言不能在函数内再声明函数（python可以，比如装饰器），但是可以在函数内调用函数（当然了），也包括调用自己。

很多情况下递归的速率并不比迭代快，反而会很慢，因为调用的开销很大，还可能导致堆栈溢出。但递归在解决某些算法问题时要比迭代容易得多。

### 6.8 函数原型

```c
#include <stdio.h>

int sqr(int n);

int main(void)
{
    int x = 10;
    printf("%d %d\n", sqr(x), x);
    return 0;
}

int sqr(int n)
{
    return n * n;
}
```

第二行的`int sqr(int n);`就叫做函数原型。当然如果函数在主函数之前定义了，那么函数定义可以充当函数原型。但那样不规范，如果是文件很大，函数很多的C程序，或者是有很多文件的程序，最好把函数原型写在前面。

**原型能使编译程序提供更强的类型检查**。虽然在C程序中不是必需的，但**在C++中是必需的**。

在C和C++中对函数原型的处理有细微但重要的区别。在C程序中，如果函数原型如下

```c
int func();
```

表示该函数的参数列表没有给出，并不代表没有参数。如果没有参数的话会在参数列表填写一个`void`。但在C++中，空参数列表即代表没有参数，所以`void`就很多余了。

### 6.9 定义可变长度的参数表

可以定义参数的类型和数量都可变的函数。为了把传递给一个函数的未知变元数告诉编译程序，我们用三个圆点结束函数形参的声明。例

```c
int func(int n, ...);
```

函数定义也可以使用这种声明。

注意，使用可变参数值的任何函数，**至少必须有一个实际的参数**。如下例是非法的

```c
int func(...); //illegal
```

### 6.10 “隐含的int”规则

在C89及以前的C版本中，如果一个函数没有声明返回值类型，则默认为`int`类型。但该特性已在C99和C++中舍弃，**强烈不建议在C89版本中省略返回值类型声明，最好还是明确声明**。

### 6.11 参数声明的老式方法和现代方法

```c
//现代方法
float func(int a, int b, char ch)
{
    /* ... */
}
//老式方法
float func(a, b, ch)
int a, b;
char ch;
{
    /* ... */
}
```

标准C已命令废弃老式的参数声明方法，但有些早期C程序还这样写，所以认识就好，**不要这样写**，而且C++中仅支持现代方法。

### 6.12 inline关键字

C99新加的用于函数的关键字`inline`，可以优化对函数的调用，后面再说。

## 第七章 结构、联合、枚举和用户定义类型

### 7.1 结构

结构是在一个名下引用的多种变量的集合，提供一种把相关数据组合到一起的方便手段。构成结构的变量成为成员，或者元素或者域。通常，结构中的诸成员都是逻辑相关的。例

```c
struct person
{
    char name[30];
    short age;
    char sex[10];
};
```

注意，**定义结尾有分号**！因为结构定义也是C语句。在这里`person`是结构标记，标识这一特定的数据结构，是结构的类型标识符。也就是说，我们定义了一种新的数据类型！

在结构定义过程中没有任何变量产生，我们声明这种新类型的变量时，可以这样

```c
struct person student;
```

这里，`student`就是一个`struct person`型的结构变量。即`person`描述结构，`student`描述对象/实例。

定义结构变量后，程序就会自动为结构的所有成员分配足够的内存。

定义结构的同时可以定义一个或多个对象。

```c
struct person
{
    char name[30];
    short age;
    char sex[10];
} student, teacher;
```

此时各个结构变量中的成员都有自己单独的内存，它们是相互独立的，不同的。改变一者不会影响另一者。

而如果只需要一个结构变量，则结构标记可以省略，如

```c
struct
{
    char name[30];
    short age;
    char sex[10];
} student;
```

结构标记和结构变量可以二选一，但必须有一。

#### 7.1.1 存取结构成员

通过圆点(.)操作符可以存取结构中的成员。例

```c
student.age = 19;
```

### 7.2 结构数组

结构最常见的用法可能就是结构数组。例如对前例定义的`person`结构，书写

```c
struct person stu_ls[50];
```

即生成组织进`stu_ls`的50个`person`型结构变量，引用时如下

```c
printf("%d", stu_ls[2].age);
```

### 7.3 向函数传递结构

#### 7.3.1 向函数传结构成员

把结构变量的一个元素传入函数时，实际上传递的是结构成员的值，相当于传递简单变量（假设成员不是字符串等复杂结构）。

>   如果是复杂结构又传递的是什么呢？

```c
struct fred
{
    char x;
    int y;
    float z;
    char s[10];
} mike;
func(mike.x);
```

如果要传递结构成员的地址，应该把`&`放在结构名前，而非具体成员名前。例

```c
func2(&mike.y);
func3(mike.s); //mike.s已经是地址了，不需要加&了
```

#### 7.3.2 向函数传递全结构

结构用作函数的变元时，用**标准值调用**规则把全结构传给函数。因此，函数内对结构内容的修改不影响原结构，即函数退出后，原结构内容不变。

用结构作变元时，必须注意类型匹配。**不仅是结构内容完全相同，结构类型名也必须相同。**因为即便是相同的内容，不同的类型名也意味着不同的结构。此处参考python中的类，即便两个类的属性方法完全相同，只要类名不同，它们就是两个不同的类。

```c
#include <stdio.h>

struct struct_type
{
    int a, b;
    char c;
};

void func(struct struct_type parm)
{
    printf("%d\n", parm.a);
}

int main(void)
{
    struct struct_type arg; //此处的结构类型必须与函数func参数里的结构类型相同
    arg.a = 100;
    func(arg);
    return 0;
}
```

### 7.4 结构指针

#### 7.4.1 定义结构指针

在结构变量名前加`*`。例

```c
struct st* p;
```

#### 7.4.2 使用结构指针

结构指针有两个主要用途：产生对函数的引用调用；生成链表和依赖动态分配的动态数据结构。

如果向函数传递的是全部的结构，则当函数调用时，需要把全部的结构数据压栈，当结构成员很多很复杂时，花销巨大。但如果只传递结构指针，就简单多了，因为压栈的只是一个结构地址，函数调用就会非常快。而且，传递指针还允许函数修改原结构内容。

在结构变量名前加`&`，得到结构变量的地址。

```c
struct st
{
    int a, b;
    char c;
} arg;
struct st* p;
p = &arg;
```

**通过结构指针访问结构成员时，必须用操作符`->`**。例

```c
p->a = 10;
printf("%d", p->a);
```

### 7.5 结构中的数组和结构

结构的成员可以是简单变量，也可以是复合类型。**在C语言中，复合类型就是数组和结构。**

结构内也可以嵌套结构。例

```c
struct employee
{
    struct addr address; //addr是另外一种结构类型，address是该结构的一个变量
    float wage;
};
```

### 7.6 联合

联合是多种变量共享的一片内存。联合提供了以多种方式解释同一位模式的方法。联合的定义与使用方法与结构几乎相同，只是把`struct`换做`union`。

比如说，定义了一个联合

```c
union ut
{
    int a;
    short b;
    char c;
} cnvt;
```

对于联合变量`cnvt`，它的大小只有4个字节。即联合变量的大小按联合内**最大的类型**算。而当我们读取`cnvt`的内存数据时，可以按`int`为单位读，也可以按`short`为单位读，也可以按`char`为单位读。即他们共享这四个字节的内存区域。

联合常用于需要频繁进行类型转换的场合。

>   虽然这样说，我目前并没有很深的理解。

### 7.7 位域

C语言具备访问字节中**位**的内设机制，成为位域。通过它可以访问单个的位。

位域必须是许多的结构或联合，它定义了以位计算的域长。位域定义的一般形式是`type name: length;`。

`type`指定位域的类型，必须是`int`、`signed`或`unsigned`（C99新增了`_BOOL`）。`length`指定位域的位数。

位域常用于分析硬设备的输入。位域变量的限制：不能取位域变量的地址；不能构造数组位域；位域变量不能跨越整数边界。而且位域对机器也有很大的依赖，不同机器中的位域顺序可能不一样。

>   同样，并不了解这个东西，估计也用不到。

### 7.8 枚举

枚举是一系列命名的整型常量。若如下定义

```c
enum coin { penny, nickel, dime, quarter, dollar } money;
```

则`penny`等只是一系列整型的别名。可以如下使用

```c
#include <stdio.h>

int main(void)
{
    enum coin { penny, nickel, dime, quarter, dollar };
    printf("%d %d\n", penny, dime); //输出0 2
    return 0;
}
```

枚举内的符号可以初始化，如

```c
enum coin { penny, nickel, dime = 100, quarter, dollar };
//penny=0,nickel=1,dime=100,quarter=101,dollar=102
```

枚举常用于定义编译程序的符号表。

>   一如既往，不懂不懂。

### 7.9 C和C++之间的重要差别

在C++中定义了结构、联合或枚举后，可以只使用类型名来定义变量，而不必在类型名前加`struct`或`union`或`enum`。比如在C++中

```c++
struct MySt {int a; char c;};
MySt st;
```

这样子完全可以，但在C中必须在`MySt`前加`struct`。当然在C++中加上也没有问题。总之移植代码时需要考虑这个问题。

### 7.10 用sizeof确保可移植性

动态分配内存时，使用`sizeof`而非手动分配。如

```c
struct s { char c; int i; double f; } s_var;
struct s* p;
p = malloc(sizeof(struct s));
```

### 7.11 typedef

C语言允许通过`typedef`为数据类型定义新名字。此时，**原数据类型并没有消除，也没有产生新的数据类型**。如

```c
typedef float balance;
balance f = 1.2f;
```

程序认为`balance`是`float`的一个别名。该特性使得依赖机器的程序更容易移植。

## 第八章 控制台I/O

### 8.1 读写字符

最简单的I/O函数是`getchar()`和`putchar()`，两者分别从键盘读一个字符或向显示屏写一个字符。两者的原型分别是

```c
int getchar(void);
int putchar(int c);
```

#### 8.1.1 getchar()的问题

在`getchar()`的原始形式中，输入先被缓冲，直到键入回车键时才返回。这就是所谓的行缓冲(line-buffer)输入。

>   具体表现可能是并不会在你输入一个字符后立即返回，而是允许你输入很多字符但只会取一个字符。
>
>   提问：下段程序中的`getchar()`为什么可以这样用？

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE* fp;
    char ch;
    if ((fp = fopen("test", "r+")) == NULL)
    {
        printf("Can't open it.\n");
        exit(1);
    }
    do
    {
        ch = getchar();
        putc(ch, fp);
    } while (ch != '$');

    return 0;
}
```

#### 8.1.2 代替getchar()的函数

`getchar()`并不能交互，很不方便，因此所有C编译程序都提供了交互函数。

>   这个“交互”到底啥意思，我暂时也没搞懂。

最常见的函数是

```c
int _getch(void);
int _getche(void);
```

它们的原型在库函数`conin.h`中创建。在Visual C++中这些函数前有下划线，但有些编译程序中没有。

这两个函数**在键击后立即返回**。前者不向屏幕回显字符，后者回显。

### 8.2 读写串

按照复杂性和功能程序，控制台I/O的下一批函数是`gets()`和`puts()`。这两个函数允许程序从控制台读和向控制台写字符串。

```c
char* gets(char* str);
int puts(const char* str);
```

但在Visual C++中貌似找不到`gets()`了。仿佛因为它不进行边界检查，已经被替代了。

`puts()`只处理字符串，所以不考虑格式转换时，使用`puts()`要比`printf()`更快，占空间更小。

### 8.4 printf()

原型

```c
int printf(const char* control_string, ...);
```

`printf`格式说明符：

| 代码 | 格式                                                         |
| ---- | ------------------------------------------------------------ |
| %c   | 字符                                                         |
| %d   | 带符号十进制整数                                             |
| %i   | 带符号十进制整数                                             |
| %e   | 科学表示（小写e表示指数部分）                                |
| %E   | 科学表示（大写E表示指数部分）                                |
| %f   | 十进制浮点数                                                 |
| %g   | 在%e或%f中则短选择                                           |
| %G   | 在%E或%f中则短选择                                           |
| %o   | 无符号八进制                                                 |
| %s   | 字符串                                                       |
| %u   | 无符号十进制整数                                             |
| %x   | 无符号十六进制（小写）                                       |
| %X   | 无符号十六进制（大写）                                       |
| %p   | 显示指针                                                     |
| %n   | 相应变元是指向整数的指针，至此已写的字符数被printf()放入其中 |
| %%   | 显示百分号                                                   |

#### 8.4.4 格式说明符%n

```c
#include <stdio.h>

int main(void)
{
    int count;
    printf("this%n is a test\n", &count);
    printf("%d\n", count);  //输出4，因为"this"有4个字符
    return 0;
}
```

#### 8.4.9 处理其他数据类型

有两个格式说明符允许`printf()`显示短和长整数，它们适用于格式说明符`d`、`i`、`o`、`u`和`x`。修饰符`l`处理长整数，修饰符`h`处理短整数。

#### 8.4.10 修饰符*和#

在`g`、`f`和`e`前加`#`时，确保出现小数点，即便没有小数位也如此。

>   浮点数难道不是自带小数位？怎么会没有小数位？

在`x`前加`#`使输出的十六进制数带有前缀`0x`。

`*`可以动态指定最小域宽和显示精度。例

```c
printf("%*.*f\n", 10, 4, 123.3);
```

表示该浮点数最小域宽为10，显示精度为4，当然10和4也可以是其它变量。

### 8.5 scanf()

Visual C++废除了`scanf()`，用`scanf_s()`代替，后者在接收字符串时需要指定字符长度。

`scanf_s()`遇到**空格**或**换行**即结束。所以它无法完整读取`hello world`。

#### 8.5.8 使用扫描集合

```c
#include <stdio.h>

int main(void)
{
    int i;
    char s[20], s2[20];
    scanf_s("%d%[abcdefg]%s", &i, s, 20, s2, 20); //输入123abcdtef
    printf("%d %s %s\n", i, s, s2); //输出123 abcd tef
    return 0;
}
```

所以`%[abcdefg]`的作用是只接受方括号内的字符，当遇到第一个不是方括号内的字符时结束。

`%[A-Z]`表示取从A到Z的字符，区分大小写，在开头加`^`表示取反。类似正则表达式。

#### 8.5.13 忽略输入

```c
#include <stdio.h>

int main(void)
{
    int a, b;
    scanf_s("%d%*c%d", &a, &b); //输入10,10
    printf("%d %d\n", a, b); //输出10 10
    return 0;
}
```

在域的格式码前放星号，使`scanf_s()`读入该域但不向任何变量赋值。即忽略了该内容。

## 第九章 文件I/O

### 9.3 流和文件

C语言的I/O系统给程序员提供了与设备无关的一致界面，即C的I/O系统在程序员和设备间提供了一级抽象。抽象被称为**流**，而实际设备则称为**文件**。

### 9.4 流

C文件系统是为适应各种设备而设计的，设备包括终端、磁盘和磁带驱动器等。各种设备的差别很大，但缓冲文件系统把各种设备都转换成称为流的逻辑设备。所有流的性质完全类似。因为流基本上与设备无关，所以能写入磁盘文件的同一函数也能写入另一类型设备，如控制台终端等。即我们不必为磁盘文件和控制台终端分别写一个输出函数了。

共有两类流：文本流和二进制流。

#### 9.4.1 文本流

文本流（text stream）是一种字符序列。在文本流中，可能依设备需要而存在某种字符翻译，比如换行符。

#### 9.4.2 二进制流

二进制流（binary stream）是一种字节序列。没有字符变换，但末尾可能会填充null符号。

>   填充null字节可能是为了恰好充满一个磁盘扇区，方便存取、提高速度。但其实我并不懂。

### 9.5 文件

C语言I/O系统的一个要点：所有流都是完全一样的，有些文件则不完全一样。

与文件相关联的每个流都有一个`FILE`类型的控制结构，用户绝对不应修改。

在C中，我们只考虑流，用单一的文件系统完成全部I/O操作。

### 9.6 文件系统基础

最常用的缓冲文件系统函数：

| 名称      | 功能                                     |
| --------- | ---------------------------------------- |
| fopen()   | 打开一个文件                             |
| fclose()  | 关闭一个文件                             |
| putc()    | 向文件写一个字符                         |
| fputc()   | 同上                                     |
| getc()    | 从文件中读一个字符                       |
| fgetc()   | 同上                                     |
| fgets()   | 从文件中读一字串                         |
| fputs()   | 写字串到文件                             |
| fseek()   | 在文件中定位于特定字节                   |
| ftell()   | 返回当前文件位置                         |
| fprintf() | 与文件的关系和printf()与控制台的关系相同 |
| fscanf()  | 与文件的关系和scanf()与控制台的关系相同  |
| feof()    | 到达文件尾时返回真值                     |
| ferror()  | 发生错误时返回真值                       |
| rewind()  | 把文件的定位指示置回文件开始处           |
| remove()  | 删除一个文件                             |
| fflush()  | 对一个文件清仓                           |

#### 9.6.1 文件指针

文件指针是贯穿缓冲I/O系统的主线。文件指针是指向定义文件操作信息的指针，信息中包括**文件的名字、状态和当前读写位置**。本质上，文件指针标识一个特定磁盘文件，被相连的流用来指导缓冲文件函数的操作。

>   可能就是用来作为文件函数的参数？

文件指针是`FILE`型指针变量，在`stdio.h`中定义。书写如下

```c
FILE* fp;
```

#### 9.6.2 打开文件

函数`fopen()`打开一个流，将该流和一个文件关联，然后返回有关的文件指针。最常见的文件是磁盘文件。原型为

```c
FILE* fopen(const char* filename, const char* mode);
```

`mode`是打开方式，具体与python相同。（当然应该是python跟C相同）

函数`fopen()`返回一个文件指针，空文件指针表示打开失败。**程序中绝对不应该更改`fopen()`返回的指针**。

在Visual C++中使用`fopen()`会提示不安全，会建议用`fopen_s()`，但这个函数用起来略麻烦一点，自行搜索吧。

#### 9.6.3 关闭文件

函数`fclose()`关闭`fopen()`打开的流，并把遗留在缓冲区的数据写入文件。多数情况下，系统都会限制同时处于打开状态的文件总数，因此打开文件前先关闭无用文件是合理的。可同时打开的文件总数查阅常量`FOPEN_MAX`，其定义在`stdio.h`中。原型

```c
int fclose(FILE* fp);
```

函数返回**零值**表示关闭成功。

#### 9.6.4 写字符

`putc()`等价于`fputc()`。实际上前者是用宏实现的。历史遗留问题，随便哪个都行。原型

```c
int putc(int ch, FILE* fp);
```

#### 9.6.5 读字符

`getc()`和`fgetc()`也是等价的。原型

```c
int getc(FILE* fp);
```

到达文件尾时函数返回`EOF`，该常量在`stdio.h`中定义。以下代码读取文本文件，并输出字符

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE* fp;
    char ch;
    if ((fp = fopen("test", "r")) == NULL)
    {
        printf("Can't open it.\n");
        exit(1);
    }
    do
    {
        ch = fgetc(fp);
        putchar(ch);
    } while (ch != EOF);
    fclose(fp);
    return 0;
}
```

#### 9.6.7 使用feof()

虽然遇到文件末尾时，`getc()`会返回`EOF`，但并不是每次返回`EOF`都意味着到达了文件尾。在二进制文件中也可能存在等于`EOF`值的整数，或者`getc()`失败也会返回`EOF`。为了明确文件是否到达尾部，C语言提供函数`feof()`，原型为

```c
int feof(FILE* fp);
```

到达文件尾时，返回**真值**，否则返回零。

#### 9.6.8 用fputs()和fgets()处理串

C语言支持函数`fputs()`和`fgets()`，向文件写读**字符串**。原型

```c
int fputs(const char* str, FILE* fp);
char* fgets(char* str, int length, FILE* fp);
```

`fputs()`出错时会返回`EOF`。`fgets()`读到**新行符**或者`length-1`个字符时结束。遇到新行符时也会把新行符作为串的一部分，而`gets()`不会。例

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE* fp;
    char ch[20];
    if ((fp = fopen("test", "r")) == NULL)
    {
        printf("Can't open it.\n");
        exit(1);
    }
    fgets(ch, 20, fp); //也就是说只读一行。
    printf("%s\n", ch);
    fclose(fp);
    return 0;
}
```

`fgets()`也会返回一个指针，如果读取失败会返回空指针，如果成功会返回`str`。

>   所以为啥参数里有字符指针了，还要返回一个字符指针呢？经测试两者完全一样啊。

#### 9.6.9 rewind()

重置文件的位置指示于文件开始处，相当于python中的`seek(0)`吧。原型

```c
void rewind(FILE* fp);
```

#### 9.6.10 ferror()

每次文件操作（比如读写操作）后都应立即调用`ferror()`，防止错误状态丢失。例如

```c
ch = getc(fp);
if (ferror(fp)) /* some statements */;
```

如果出错，返回**真值**。原型

```c
int ferror(FILE* fp);
```

#### 9.6.11 删除文件

删除指定文件，成功返回**零**。原型

```c
int remove(const char* filename);
```

#### 9.6.12 对流清仓

`fflush()`对输出流清仓，把输出流上的内容清入文件。原型

```c
int fflush(FILE* fp);
```

把缓冲的全部数据写到`fp`指定的文件中。如果用空指针作变元调用`fflush()`，则所有用于输出的打开文件都被清仓。成功返回**零值**。

>   啥叫输出流？

### 9.7 fread()和fwrite()

两个函数允许读写各种类型的数据块。原型

```c
size_t fread(void* buffer, size_t num_bytes, size_t count, FILE* fp);
size_t fwrite(const void* buffer, size_t num_bytes, size_t count, FILE* fp);
```

变元`count`指定读写多少项，每项长度等于`num_bytes`。两个函数返回读写入的项数，出错时可能跟`count`不同。

#### 9.7.1 使用fread()和fwrite()

注意使用`sizeof`确定每种数据的长度。同时**最好检验两个函数的返回值，以确保读写正确**。

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE* fp;
    double d = 123.4;
    double f;
    if ((fp = fopen("test", "wb+")) == NULL) //注意带着加号，保证可写可读，但b应该是可选的
    {
        printf("Can't open it.\n");
        exit(1);
    }
    fwrite(&d, sizeof(double), 1, fp); //注意取地址，因为参数是指针
    rewind(fp);
    fread(&f, sizeof(double), 1, fp); //此处都没检验返回值，因为相对简单
    printf("%f\n", f);
    fclose(fp);
    return 0;
}
```

两个函数最重要的应用之一是读写用户定义的数据类型，特别是结构。

### 9.8 fseek()和随机存取I/O

原型

```c
int fseek(FILE* fp, long numbytes, int origin);
```

`numbytes`是从原点`origin`到新位置的字节数，`origin`取`stdio.h`中定义的三个宏之一：`SEEK_SET`、`SEEK_CUR`、`SEEK_END`。它们分别指文件的开始位置，当前位置，结束位置。

通过`fseek()`可以以任意类型的长度为单位，寻找该类数据项，比如

```c
fseek(fp, 9*sizeof(int), SEEK_SET);
```

寻找从文件开始的第10个整型。成功返回**零值**。

使用`ftell()`可以确定文件的当前位置，原型

```c
long ftell(FILE* fp);
```

成功返回当前位置，失败返回-1。

一般来说，程序员仅对二进制文件使用随机存取，因为文本文件可能存在字符转换。但如果以`SEEK_SET`为原点位置，也可以对文本文件使用`fseek()`。

对于包含文本的文件，随即存取没啥约束，但对于不包含文本但作为文本文件打开的文件，有约束。

>   挺绕的，看着理解吧。我还是觉得用python处理文件简单些。

### 9.9 fprintf()和fscanf()

除了操作对象不同外，跟`printf()`和`scanf()`完全一样。原型

```c
int fprintf(FILE* fp, const char* control_string, ...);
int fscanf(FILE* fp, const char* control_string, ...);
```

通过以下程序了解其用法吧。

```c++
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    FILE* fp;
    char s[20];
    int t;
    if ((fp = fopen("test", "w+")) == NULL)
    {
        printf("Can't open it.\n");
        exit(1);
    }
    printf("Enter a string and a number: ");
    fscanf(stdin, "%s %d", s, &t); //从键盘获取
    fprintf(fp, "%s %d", s, t); //写入文件
    rewind(fp);
    fscanf(fp, "%s %d", s, &t); //从文件获取
    fprintf(stdout, "%s %d", s, t); //写入控制台
    fclose(fp);
    return 0;
}
```

这两个函数在处理格式化数据时很方便，但效率不是很高，开销要比`fread()`和`fwrite()`大。

### 9.10 标准流

一个C程序开始运行时，自动打开三个流：标准输入`stdin`、标准输出`stdout`和标准错误`stderr`。

标准流是文件指针，所以可以用于文件函数中参数类型是`FILE*`的地方。如

```c
char s[30];
fgets(s, 80, stdin);
```

表示从标准输入，即键盘接受字符串。这样用法要比`gets`安全，因为它限制读入的字符数量，防止溢出，但麻烦是`fgets()`不删除末尾的新行符，需要手动删除并替换为`\0`。

#### 9.10.1 控制台I/O的连接

C语言对控制台I/O和文件I/O基本不加区别，文件也是设备。所以很多操作相通。比如可以理解`stdin`和`stdout`为特殊的文件。

#### 9.10.2 用freopen()重定向标准流

原型

```c
FILE* freopen(const char* filename, const char* mode, FILE* stream);
```

>   关于重定向介绍不多，也没搞懂，可能是在命令行里用的，以后再单独研究吧。

## 第十章 预处理程序和注释

### 10.1 预处理程序

ANSI标准定义的预处理程序有以下几种

```c
#if
#ifdef
#ifndef
#else
#elif
#endif
#define
#undef
#line
#error
#pragma
#include
```

每条预处理指令必须**独占一行**。

### 10.2 #define

定义的宏名字可以在其它宏定义中使用。

```c
#define ONE 1
#define TWO ONE+ONE
```

#### 10.2.1 定义类函数宏

宏名字可以有变元。如

```c
#include <stdio.h>
#define ABS(a) (a) < 0 ? -(a) : (a)

int main(void)
{
    printf("%d\n", ABS(-1));
    return 0;
}
```

这一形式叫**类函数宏**。优点是提高了代码速度，消除了调用开销，但如果类函数宏的尺寸很大，缺点就是因代码复制而增加了程序规模。因为程序会把所有出现宏的地方都用实际值替换掉。

### 10.3 #error

该指令强制编译程序停止编译，主要用于程序调试。一般形式

```c
#error error_message
```

宏串不用双引号包围。

>   不知道怎么用。

### 10.6 #undef

`#undef`删除前面定义的宏名字。

### 10.7 使用defined

`#if defined VAR`等同于`#ifdef VAR`，注意都需要`#endif`来结束条件编译。

使用`defined`的一个原因是，它允许由`#elif`语句确定的宏名字存在。

### 10.8 #line

`#line`改变`__LINE__`和`__FILE__`的内容。两者都是编译程序预定义的标识符。前者的内容是当前被编译代码行的**行号**，后者是当前被编译源文件的**文件名**。`#line`的一般形式

```c
#line number "filename"
```

`number`是正整数，`filename`是合法文件标识符。两者可只定义一个或者两个都定义。主要用于调试和特殊应用。

>   不知道有啥用。

### 10.9 #pragma

>   最开始了解到的是`#pragma once`可以防止嵌套文件进入死循环，但貌似还有很多其它用途，用到的时候再研究。

### 10.10 预处理操作符#和##

>   比较麻烦，不多解释，一般用不到，如果用到了，自行搜索。

### 10.11 预定义宏

C规范了五个固有的预定义宏。分别是

```c++
__LINE__
__FILE__
__DATE__ //源代码编译成目标码的时间，形如hour:minute:second
__TIME__ //源代码编译成目标码的日期，形如month/day/year
__STDC__ //如果是十进制常数1，则表示编译程序的实现符合标准C
```

### 10.12 注释

C89只有一种风格的注释，即多行注释。**多行注释不能嵌套**。C99（及C++）支持单行注释。

虽然技术上C89不支持单行注释，但很多编译程序还是接受它们。

