---
layout: post
title: 《C语言大全第4版》笔记
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

