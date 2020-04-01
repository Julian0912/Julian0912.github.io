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

如果使用指针修改，虽然目标字符可以修改成功，但修改部分以后的字符就无法输出了，暂时不明白原因，以后或许会回来说明。

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



