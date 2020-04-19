---
layout: post
title: 生命游戏
categories: Hobbies
description: 
keywords: 
---

*我第一次听说生命游戏是在高中的时候，那时候一下子就被它的奇妙规则给吸引了，但奈何当时条件有限，没学编程，也没有电脑，就只能纯手工在纸上画，最后把纸画破了，还是没画对。因为总会有遗漏。这次偶然在YouTube上看到@s0lly的视频，用Excel画生命游戏，顿时来了兴趣，就把过程记了下来。*

<!--more-->

首先打开Excel，把部分表格调成正方形，范围随便定，这里打算用20\*20的表格演示，因此范围稍大于20\*20。

选中n列表格后，**按住Shift**可以统一调整宽度，这里调为25像素，行表格也一样。然后添加边框。将此工作表改名为Current State。

<img src="/images/posts/GOF/GL01.png" alt="GL01" style="zoom:50%;" />

随便在表格内填充1和0，可以智能快速填充，也可以复制粘贴。

<img src="/images/posts/GOF/GL02.png" alt="GL02" style="zoom:50%;" />

选中20\*20的表格范围，在**条件格式**选择**色阶**中的绿-白色阶。

<img src="/images/posts/GOF/GL03.png" alt="GL03" style="zoom:50%;" />

然后继续**管理规则**，双击规则，将规则编辑至如下所示

<img src="/images/posts/GOF/Gl04.png" alt="GL04" style="zoom:50%;" />

即最小者和最大值都用选择**数字**，然后分别调为0和1，最大值的颜色改为**黑色**。

继续选中表格，右键**设置单元格格式**，在**数字**一栏下选中**自定义**，然后在**类型**里填入一对**英文双引号**，这样表格就不会显示0和1了，但其值依然是0和1。

<img src="/images/posts/GOF/GL05.png" alt="GL05" style="zoom:50%;" />

<img src="/images/posts/GOF/GL06.png" alt="GL06" style="zoom:50%;" />

接下来，复制两个相同的工作表，其名称分别为Neighbours和New State。

<img src="/images/posts/GOF/Gl07.png" alt="GL07" style="zoom:50%;" />

然后在**Neighbours**工作表中，选择条件格式中的**清除规则**，清除整个工作表的规则，同时把表格的单元格格式改为**通用格式**，即重新显示0和1。

<img src="/images/posts/GOF/GL08.png" alt="GL08" style="zoom:50%;" />}

然后在**Neighbours**工作表中，选中20\*20表格范围的左上角第一个单元格，输入`=SUM('Current State'!A1:C3)-'Current State'!B2`，即计算该单元格对应的**Current State**工作表中的单元格周围的数值总和，同时减去自身的值，即计算了周围**8**个单元格的值的和。然后将该公式智能应用到所有20\*20的单元格内（拖拽）。

<img src="/images/posts/GOF/Gl09.png" alt="GL09" style="zoom:50%;" />

<img src="/images/posts/GOF/Gl10.png" alt="GL10" style="zoom:50%;" />

接下来，在**New State**工作表中，同样选中表格范围中的左上角的第一个表格，输入`=IF('Current State'!B2=1,IF(OR(Neighbours!B2=2,Neighbours!B2=3),1,0),IF(Neighbours!B2=3,1,0))`。

该语句的含义如下表所示

|                                  | 对应**Current State**表中的值为1 | 对应**Current State**表中的值为0 |
| -------------------------------- | -------------------------------- | -------------------------------- |
| 对应**Neighbours**表中的值为2    | 值为1，即存活                    | 值为0，即死亡                    |
| 对应**Neighbours**表中的值为3    | 值为1，即存活                    | **值为1，即存活**                |
| 对应**Neighbours**表中的值为其它 | 值为0，即死亡                    | 值为0，即死亡                    |

或者如下所示

对应**Current State**表中的值为1：

*   如果对应**Neighbours**表中的值为2或3：
    *   值为1，即存活
*   否则：
    *   值为0，即死亡

否则：

*   如果对应**Neighbours**表中的值为3：
    *   值为1，即存活
*   否则：
    *   值为0，即死亡

<img src="/images/posts/GOF/Gl11.png" alt="GL11" style="zoom:50%;" />

把该公式扩展到所有20\*20的表格（拖拽）。如下所示

<img src="/images/posts/GOF/Gl12.png" alt="GL12" style="zoom:50%;" />

生命已完成了第一个周期！

接下来只需要把**New State**工作表中的表格复制，然后回到**Current State**工作表中，以**值**的方式覆盖粘贴原表格，注意，粘贴方式一定选择第二项，**只粘贴值！**

<img src="/images/posts/GOF/Gl13.png" alt="GL13" style="zoom:50%;" />

此时啥也不要动，只需要继续按**F4**就可以重复刚才的复制粘贴操作了！

生命游戏一个周期一个周期地运行起来了！

<img src="/images/posts/GOF/Gl14.png" alt="GL14" style="zoom:50%;" />

理论上说它总会遇到一个不变周期，或者循环周期，或者跑到边界外去了。

总之，可以继续扩大表格范围（当然还是拖拽），甚至可以改写规则！

**Do Enjoy Yourself!**