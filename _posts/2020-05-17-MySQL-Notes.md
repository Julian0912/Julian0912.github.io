---
layout: post
title: MySQL学习笔记
categories: Programming
description: 
keywords: 
---

*一直拖了好久的数据库学习，本来是觉得一直用不到所以就没学，但发现Navicat挺好的，所以还是学一学吧。*

<!--more-->

##### 例1：登录

进入MySQL的bin目录，输入以下登录MySQL（若MySQL没启动则输入net start mysql，或者输入services.msc手动开启）

```
~\bin>mysql -u root -p <密码>
```

##### 例2：查看已有的数据库

```mysql
SHOW DATABASES;
```

注意：**分号不能漏！**`DATABASES`是复数！

##### 例3：创建新数据库

```mysql
CREATE DATABASE test_db;
```

`test_db`是一个新数据库的名字。

注意：数据库名字**不区分**大小写！`DATABASE`是单数！

##### 例4：删除数据库

```mysql
DROP DATABASE test_db;
```

如果想在删除之前先查看存不存在，可以

```mysql
DROP DATABASE IF EXISTS sd_db;
CREATE DATABASE sd_db CHARACTER SET utf8mb4;
```

第二句表示设置数据库编码为utf8。

##### 例5：选择数据库

```mysql
USE test_db;
```

##### 例6：创建数据表

```mysql
CREATE TABLE user_info(
    id INT NOT NULL,
    name VARCHAR(10) NOT NULL,
    sex CHAR(2),
    age INT
);
```

`CHAR`和`VARCHAR`地区别后面有，这里简单说的话，就是前者是定长字符串，若输入不足长度则用空格填充，在输出时删去尾部空格；后者是变长字符串，长度不足不会用空格填充，输出时也会保留尾部空格。前者效率高，常用于长度变化不大且需要快速查询的数据，后者效率偏低，会在长度超出限制时将类型强制转换成`TEXT`型。

##### 例7：数据库数据类型

<img src="/images/posts/MySQL_N/MySQL数值类型.png" alt="numbers" style="zoom:60%;" />

数值类型常见的有`INT`，`FLOAT`，`DOUBLE`。

<img src="/images/posts/MySQL_N/MySQL字符串类型.png" alt="strings" style="zoom:60%;" />

常用的字符串类型有`CHAR`，`VARCHAR`。

<img src="/images/posts/MySQL_N/MySQL日期时间类型.png" alt="datetime" style="zoom:60%;" />

日期时间类型中常用的有`DATE`，`TIME`，`DATETIME`，其中`DATETIME`最常用。

##### 例8：Navicat新建查询

<img src="/images/posts/MySQL_N/navi00.png" alt="navi00" style="zoom:60%;" />

点击**查询**，点击**新建查询**，

<img src="/images/posts/MySQL_N/navi01.png" alt="navi01" style="zoom:60%;" />

输入命令，点击**运行**，

<img src="/images/posts/MySQL_N/navi02.png" alt="navi02" style="zoom:60%;" />

运行前可以先保存，名称一般为数据库名称。当然如果有些固定的很长的查询步骤可以单独保存成其它名字的文件。

运行后会显示反馈信息，报错与否就知道了。

如果运行后没有发现新的数据表，则在**表**处右键刷新一下。

<img src="/images/posts/MySQL_N/navi03.png" alt="navi03" style="zoom:60%;" />

##### 例9：主键

```mysql
CREATE TABLE test(
    id INT NOT NULL PRIMARY KEY,
    name VARCHAR(10) NOT NULL
);
```

主键以`PRIMARY KEY`标识，一个数据表只能有一个主键，且主键的值都不重复。一般来说，一个主键对应一个字段。（一个主键也可以对应多个字段）

##### 例10：Navicat设计表/命令行描述表

<img src="/images/posts/MySQL_N/navi04.png" alt="navi04" style="zoom:60%;" />

选择一个数据表右键点击**设计表**，可以可视化修改字段类型等信息。

在命令行中，使用关键字`DESCRIBE`或`DESC`查看字段属性。

```mysql
DESC user_info;
```

##### 例11：插入数据

```mysql
INSERT INTO user_info(id, uname, sex, age) VALUES (1, 'fmy', 'm', 20);
```

注意：类型要一一对应。

~~如果类型不对应，MySQL会进行强制转换，如果不能强制转换，则替换为0。不能强制转换的情况一般发生在字符串转数值的时候。~~

在Navicat里类型不对应不会添加成功。会报错。

命令行里也是。

插入数据有个简单方式，**如果插入所有字段，则字段可以省略不写**。

```mysql
INSERT INTO user_info VALUES (7, 'vm3', 'f', 31);
```

**但不建议这样用**，因为容易疏漏。

对于每次都插入**相同数量和类型**的数据，可以这样简写：

```mysql
INSERT INTO user_info(uname, sex, age) VALUES('fmy', 'm', 21), ('fzr', 'm', 44), ('fmx', 'f', 15), ('xly', 'f', 45);
```

##### 例12：添加注释

```
-- this is a comment
```

注意：两个短杠后**有空格**。

##### 例13：删除数据

```mysql
DELETE FROM user_info WHERE uname = 'vm';
```

`WHERE`后跟各种**条件**。

如果没加条件，则表示删除所有信息。

##### 例14：查询数据

```mysql
SELECT uname, age FROM user_info;
```

特殊的，如果要查询一个数据表中的所有字段，可以使用`*`代替，即

```mysql
SELECT * FROM user_info;
```

但尽量不使用`*`，因为它**效率要低**。一般需要什么字段，就查询什么字段。

查询语句可以在后面添加`WHERE`条件。

```mysql
SELECT * FROM user_info WHERE age > 20;
```

##### 例15：更新数据

```mysql
UPDATE user_info SET sex = 'm' WHERE uname = 'vm3';
```

如果修改信息本身就类似于`SET uname='vm4' WHERE uname='vm3'`。

如果不加条件，则表示修改所有字段。

如果一次修改多个字段，则

```mysql
UPDATE user_info SET sex = 'f', age = 65 WHERE uname = 'vm3';
```

##### 例16：AVG函数

```mysql
SELECT AVG(age) FROM user_info;
```

求`age`字段的平均值。本质上也是一个**查询语句**，所以用`SELECT`。

如果需要给结果附一个别名，则可以

```mysql
SELECT AVG(age) AS 'Average Age' FROM user_info;
```

**如果别名只有一个单词，可以不加引号。**而且推荐只用一个单词，若包含多个则使用驼峰命名法或下划线分隔。

同样后面可以跟条件。

```mysql
SELECT AVG(age) AS 'Average Age of Man' FROM user_info WHERE sex = 'm';
```

其它常用函数有`COUNT()`，`MAX()`，`MIN()`，`SUM()`等。

##### 例17：子查询

```mysql
SELECT * FROM user_info WHERE age = (SELECT MAX(age) FROM user_info);
```

条件查询可以这样写，这种叫子查询，即根据返回的信息确定查询条件。

##### 例18：模糊查询

```mysql
SELECT * FROM user_info WHERE uname LIKE 'f%';
```

注意使用`LIKE`关键字。`%`表示任意0个或多个字符，`_`表示一个字符。

模糊查询也可以用逻辑运算。

```mysql
SELECT * FROM user_info WHERE uname LIKE '%m%' AND age < 100;
```

##### 例19：范围查询

```mysql
SELECT * FROM user_info WHERE age BETWEEN 20 AND 30;
```

这是一个连续查询，使用关键字`BETWEEN`和`AND`。

注意：**包含边界**。

```mysql
SELECT * FROM user_info WHERE age IN (15, 25, 35, 45);
```

这是一个不连续查询。反向的话在`IN`前加`NOT`。

注意：一般括号里不会写固定的值，而是其它SQL语句，即通过其它语句返回的值来限定条件。即子查询。

```mysql
SELECT * FROM user_info WHERE sex IS NULL;
```

这是一个空判断。反向的话在`IS`后加`NOT`。

##### 例20：自增

```mysql
CREATE TABLE stu_info(
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    stu_name VARCHAR(20) NOT NULL,
    stu_num CHAR(8),
    sex CHAR(2),
    phone CHAR(11),
    birthday DATE
);
```

如同id这种字段，通常连续且不能重复，也不要有间隔，所以设置为自增最好。

关键字`AUTO_INCREMENT`，官方术语叫**标识列**。

在插入时，自增字段可以不写，建议不要手动写

```mysql
INSERT INTO stu_info(stu_name, stu_num, sex, phone, birthday)
VALUES ('囡囡', '20180007', '女', NULL, '2002-03-04');
```

注意，默认情况下，哪怕标识列中的某个值已经删除，它还是会占据位置。比如删除了id为6的记录，下一条记录自增时还是会从7开始。

##### 例21：外键

```mysql
CREATE TABLE ss_info(
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    stu_id INT,
    sub_id INT,
    FOREIGN KEY(stu_id) REFERENCES stu_info(id),
    FOREIGN KEY(sub_id) REFERENCES sub_info(id)
);
```

外键用于链接两个有关系的表，一个是主表，另一个是从表。像上一个，表`stu_info`是主表，`ss_info`是从表。

具体而准确的解释可以自行搜索。

注意：若想这样使用外键，需要设置MySQL的配置文件`my.ini`中

```ini
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

**外键约束**会在添加的外键不合理时报错。

比如此处`ss_info`表中的`stu_id`字段的值必须是`stu_info`表中存在的`id`字段值，若不存在则会添加失败并报错。

如果要删除主表的某个数据，则必须先把从表中相关的外键删掉，才能在主表中删除成功。

注意：从表的外键可以是任何字段，但链接到主表的字段**必须是主键**。

##### 例22：排序

```mysql
SELECT name, age, salary, phone FROM employee ORDER BY salary DESC;
```

`ORDER BY`关键字可以用来排序，默认升序`ASC`，可以指定为降序`DESC`。

##### 例23：分组查询

```mysql
SELECT sub_type, COUNT(sub_name) AS sub_count FROM sub_info GROUP BY sub_type;
```

根据`sub_type`的种类分类，计算每类`sub_name`的数量。

实际上这里的`sub_name`可以是任何字段，意义都是每种`sub_type`分类的数量。换做`*`也是可以的，但最好还是写个具体的字段。

所以可以看出来，`GROUP BY`一定要与聚合函数一起使用。聚合函数即之前说过的四个常用函数。

