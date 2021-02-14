[学习视频](https://www.bilibili.com/video/BV1Vt411z7wy?p=8&spm_id_from=pageDriver)

# MySQL安装、基本操作、建表约束

版本	Windows 10 专业版
版本号	20H2
安装日期	‎2021/‎1/‎1
操作系统版本	19042.789

## 一、优雅地在windows终端中操作MySQL

最终效果图：

![PowerShell](C:\Users\srxh\Desktop\DataBaseSystem\pic\PowerShell.png)

### 1. 安装MySQL

官网链接:first_quarter_moon:：[MySQL :: MySQL Downloads](https://www.mysql.com/downloads/)

### 2. 安装Windows Terminal

Windows自带的的cmd和power shell很丑，可以在Microsoft Store中搜索Windows Terminal下载安装。这一命令行工具是一款新式、快速、高效、强大的终端应用程序，适用于命令行工具和命令提示符，PowerShell和WSL等Shellu用户。主要功能包括多个选项卡、窗格、Unicode和UTF-8字符支持，GPU加速文本渲染引擎以及自定义主题、样式和配置。

[microsoft/terminal: The new Windows Terminal and the original Windows console host, all in the same place! (github.com)](https://github.com/microsoft/terminal)

### 3. 将MySQL自带的命令行工具所在目录添加到系统变量中

安装好MySQL后，会有命令行工具：MySQL Command Line Client。右击打开其所在位置即可。默认安装路径安装得到的MySQL的命令行工具的目录为`C:\Program Files\MySQL\MySQL Server 5.6\bin`

然后将其添加到系统变量中即可。

### 4. 在打开MySQL服务器的前提下进入terminal

- 登录：键入`mysql -u root -p`
- 输入密码
- 接下来就可以使用命令行工具进行操作数据库了！

## 二、基本操作--命令行工具操作数据库

### 登录MySQL

**输入**

```mysql
mysql -u root -p;
```

**返回**

```mysql
Enter password: 
```

### 查询数据库服务器中所有的数据库

**输入**

```mysql
show databases;
```

**返回**

```mysql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_database        |
| mysql              |
| performance_schema |
| sakila             |
| test               |
| world              |
+--------------------+
7 rows in set (0.00 sec)
```

### 选定一个数据库

```mysql
use 数据库名;
```

**输入**

```mysql
use world;
```

**返回**

```mysql
Database changed
```

### 显示选定的数据库中的所有表格

**输入**

```mysql
show tables;
```

**返回**

```mysql
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
3 rows in set (0.00 sec)
```

### 展示选定数据库的某一表格

```mysql
select * from 表格名;
```

**输入**

```mysql
select * from countrylanguage;
```

**返回**

```mysql
+-------------+---------------------------+------------+------------+
| CountryCode | Language                  | IsOfficial | Percentage |
+-------------+---------------------------+------------+------------+
| ABW         | Dutch                     | T          |        5.3 |
| ABW         | English                   | F          |        9.5 |
| ABW         | Papiamento                | F          |       76.7 |
| ABW         | Spanish                   | F          |        7.4 |
| AFG         | Balochi                   | F          |        0.9 |
| AFG         | Dari                      | T          |       32.1 |
.....................................................................
.....................................................................
| ZMB         | Tongan                    | F          |       11.0 |
| ZWE         | English                   | T          |        2.2 |
| ZWE         | Ndebele                   | F          |       16.2 |
| ZWE         | Nyanja                    | F          |        2.2 |
| ZWE         | Shona                     | F          |       72.1 |
+-------------+---------------------------+------------+------------+
```

### 展示选定数据库的某一表格中的某几行

```mysql
select * from 表格名 where 表头=某字段;
```

**输入**

```mysql
select * from countrylanguage where CountryCode = 'ZWE'
```

**返回**

```mysql
+-------------+----------+------------+------------+
| CountryCode | Language | IsOfficial | Percentage |
+-------------+----------+------------+------------+
| ZWE         | English  | T          |        2.2 |
| ZWE         | Ndebele  | F          |       16.2 |
| ZWE         | Nyanja   | F          |        2.2 |
| ZWE         | Shona    | F          |       72.1 |
+-------------+----------+------------+------------+
4 rows in set (0.02 sec)
```

### 退出

**输入**

```mysql
exit;
```

**返回**

```mysql
Bye
```

### 在数据库服务器中创建数据库

```mysql
create database 数据库名;
```

**输入**

```mysql
create database mytest;
```

**返回**

```mysql
Query OK, 1 row affected (0.01 sec)
```

### 如何在一个数据库中创建数据表？

之前新建了一个数据库，使用`use mytest`选中这个数据库之后，用命令`show tables`展示数据库中的数据表，返回`Empty set (0.00 sec)`表示其中并没有数据表。下面就可以在这个数据库中创建一个数据表。

```mysql
creat table 表格名 (字段名 字段类型...)
```

**输入**

```mysql
creat table pet(
	name varchar(20),
	owner varchar(20),
	sepcies varchar(20),
	sex char(1),
	birth date,
	death date);
```

**返回**

```mysql
Query OK, 0 rows affected (0.14 sec)
```

然后输入`show tables`查询数据库中的表格，返回如下

```mysql
+------------------+
| Tables_in_mytest |
+------------------+
| pet              |
+------------------+
1 row in set (0.00 sec)
```

### 查看数据表的结构

在选中一个数据库的前提下，要显示其中某一数据表的结构，可以使用以下语句

```mysql
describe 数据表名;
```

**输入**

```mysql
describe pet;
```

**返回**

```mysql
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.06 sec)
```

### 向数据表中添加记录

```
insert into 数据表名 values(一系列值)
```

**输入**

```mysql
insert into pet values('Puffball', 'Diane', 'hamster', 'f', '1999-03-30', NULL);
```

**返回**

```mysql
Query OK, 1 row affected (0.02 sec)
```

**查看当前表**

```mysql
+----------+-------+---------+------+------------+-------+
| name     | owner | species | sex  | birth      | death |
+----------+-------+---------+------+------------+-------+
| Puffball | Diane | hamster | f    | 1999-03-30 | NULL  |
+----------+-------+---------+------+------------+-------+
1 row in set (0.00 sec)
```

继续插入`insert into pet values('旺财','周星驰', '狗', '公','1999-01-10',NULL);`，然后查询数据表得到如下结果：

```mysql
+----------+--------+---------+------+------------+-------+
| name     | owner  | species | sex  | birth      | death |
+----------+--------+---------+------+------------+-------+
| Puffball | Diane  | hamster | f    | 1999-03-30 | NULL  |
| 旺财     | 周星驰 | 狗      | 公   | 1999-01-10 | NULL  |
+----------+--------+---------+------+------------+-------+
2 rows in set (0.01 sec)
```

然后又向数据表中添加一系列数据，最终执行`select * from pet`得到如下结果：

```mysql
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Puffball | Diane  | hamster | f    | 1999-03-30 | NULL       |
| 旺财     | 周星驰 | 狗      | 公   | 1999-01-10 | NULL       |
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1989-05-13 | NULL       |
| Buffy    | Harold | dog     | f    | 1994-03-17 | NULL       |
| Fang     | Benny  | dog     | f    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
| Slim     | Benny  | sanke   | m    | 1996-04-29 | NULL       |
| Puffball | Diane  | hamster | f    | 1996-04-29 | NULL       |
+----------+--------+---------+------+------------+------------+
```

### 删除数据表中的一行数据

```mysql
delete from 数据表名 where 字段名 = 某值;
```

**输入**

```mysql
delete from pet where name = '旺财';
```

**返回**

```
Query OK, 1 row affected (0.01 sec); 
```

这时通过`slect * from pet`查询数据表，返回结果如下：

```mysql
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Puffball | Diane  | hamster | f    | 1999-03-30 | NULL       |
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1989-05-13 | NULL       |
| Buffy    | Harold | dog     | f    | 1994-03-17 | NULL       |
| Fang     | Benny  | dog     | f    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
| Slim     | Benny  | sanke   | m    | 1996-04-29 | NULL       |
| Puffball | Diane  | hamster | f    | 1996-04-29 | NULL       |
+----------+--------+---------+------+------------+------------+
10 rows in set (0.00 sec)
```

为了以下学习方便，这里将删除的一行数据重新插入`insert into pet values('旺财','周星驰','狗','公','1999-01-10',NULL)`

### 如何修改数据

```
update 数据表名 set 要修改的字段名=修改为的值 where 某一字段名=某一值(可以唯一确定要修改的一行)
```

**输入**

```mysql
update pet set name='旺旺财' where owner='周星驰'；
```

**返回**

```mysql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

这时查询表格得到如下结果：

```mysql
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Puffball | Diane  | hamster | f    | 1999-03-30 | NULL       |
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1989-05-13 | NULL       |
| Buffy    | Harold | dog     | f    | 1994-03-17 | NULL       |
| Fang     | Benny  | dog     | f    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
| Slim     | Benny  | sanke   | m    | 1996-04-29 | NULL       |
| Puffball | Diane  | hamster | f    | 1996-04-29 | NULL       |
| 旺旺财   | 周星驰 | 狗      | 公   | 1999-01-10 | NULL       |
+----------+--------+---------+------+------------+------------+
11 rows in set (0.00 sec)
```

## Mysql建表时的约束

### 主键约束

唯一确定一张表中的一条记录，也就是我们通过给某个字段添加约束，就可以时的该字段**不重复**且**不为空**。

#### 创建单个主键

```mysql
create table 数据表名 (字段名 字段类型 primary key...)
```

**输入**

```mysql
create table user (id int(11) primary key, name varchar(20));
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
```

插入一组数据：

```mysql
insert into user values(1,'张三');
```

之后再插入：

```mysql
insert into user values(1,'李四');
```

会报错：

```mysql
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'
```

> 说明`id`这一字段受到主键约束，不有重复的id

尝试再插入如下：

```mysql
insert into user values(2,'张三');
```

返回如下，成功插入：

```mysql
Query OK, 1 row affected (0.00 sec)
```

> 说明`name`并不受到主键约束，可以重复

然后尝试插入如下：

```mysql
insert into user values(NULL,'张三');
```

报错如下：

```mysql
ERROR 1048 (23000): Column 'id' cannot be null
```

> 说明`id`受到主键约束，不能为空

#### 组合主键

> 组合主键

```
create table 数据表名 (字段名 字符类型...primary key(字段名1...))
```

**输入**

```mysql
create table user2 (id int, name varchar(20), password varchar(20), primary key(id,name));
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
```

分别尝试如下插入语句：

插入：

```mysql
insert into user2 values(1, '张三', '123');
```

返回

```mssql
Query OK, 1 row affected (0.01 sec)
```

插入：

```mysql
insert into user2 values(1, '张三', '123');
```

返回

```mysql
ERROR 1062 (23000): Duplicate entry '1-张三' for key 'PRIMARY'
```

插入

```mysql
insert into user2 values (2, '张三', '123');
```

返回

```mysql
Query OK, 1 row affected (0.00 sec)
```

> 组合主键约束只要保证有一个字段的属性值不同即可。

又经过一系列尝试，发现受到主键约束的联合主键任何一个字段（`name`和`id`字段）的属性值都不能为NULL

#### 创建表之后给某个字段添加主键约束

##### 修改表结构添加主键

```mysql
alter table 表格名 add primary key (字段名)
```

**输入**

```mysql
alter table user4 add primary key(id);
```

**返回**

```mysql
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

查询当前表结构返回如下

```mysql
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| id    | int(11)  | NO   | PRI | 0       |       |
| name  | char(20) | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
2 rows in set (0.03 sec)
```

> 可以添加组合主键：`alter table 表格名 add primary key(字段名1, 字段名2...)`

> **Warning**：不可以添加一个主键后又添加一个主键
>
> ```mysql
> alter table user4 add primary key(name);
> alter table user4 add primary key(id);
> ```
>
> 会报错`ERROR 1068 (42000): Multiple primary key defined`

##### 通过修改字段添加主键

```mysql
alter table 表格名 modify 字段名 字段类型 primary key;
```

**输入**

```mysql
alter table user4 modify id int primary key;
```

**返回**

```mysql
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

#### 删除主键

```mysql
alter table 表格名 drop primary key;
```

> 注意：这个语句会删掉表格中所有的主键！

### 自增约束

自增约束和主键约束结合在一起，可以只指定其他字段的属性值，而受到自增约束和主键约束的某个字段会自动生成一个编号

```mysql
create table 数据表名 (字段名 字段类型... primary key auto_increment)
```

**输入**

```mysql
create table user3 (id int primary key auto_increment, name varchar(20));
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
```

进行如下插入操作

```mysql
insert into user3 (name) values('张三')；
insert into user3 (name) values('李四')；
```

查询当前数据表返回如下

```mysql
+----+------+
| id | name |
+----+------+
|  1 | 张三 |
|  2 | 李四 |
+----+------+
2 rows in set (0.00 sec)
```

### 唯一约束

> 约束修饰的字段的值不可以重复

#### 创建表格的时候添加唯一约束

##### 直接在需要添加的字段后面添加`unique`

```mysql
create table user6 (id int unique, name varchar(20));
```

> 注意：如果此时在多个字段后都添加`unique`，则不会形成联合的唯一约束，任何一个字段出现重复都会报错。

**查看当前表格**

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  | UNI | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.03 sec)
```

**输入**

```mysql
 insert into user6 values(1,'zhangsan');
```

**返回**

```mysql
Query OK, 1 row affected (0.00 sec)
```

**输入**

```mysql
insert into user6 values(1,'lisi');
```

**返回**

```mysql
ERROR 1062 (23000): Duplicate entry '1' for key 'id'
```

**输入**

```mysql
 insert into user6 values(2,'zhangsan');
```

**返回**

```mysql
ERROR 1062 (23000): Duplicate entry 'zhangsan' for key 'name'
```

##### 定义好字段后在最后添加需要唯一约束的字段

> 如果这时候添加多个字段，则为联合的唯一约束，只要这些字段都相同才会报错。
>
> 在删除的时候，使用语句`alter table 表格名 drop index 定义的时候出现的第一个字段名`就可以删除掉整个联合唯一约束

```
create table user7(id int, name varchar(20), unique(id, name));
```

**查看当前表格**

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  | MUL | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.03 sec)
```

接下来插入如下语句都不会报错

```mysql
insert into user7 values(1,'zhangsan');
insert into user7 values(2,'zhangsan');
insert into user7 values(1,'lisi');
```

#### 创建好表格后添加唯一约束

首先创建一个没有任何约束的空表：

```
create table 数据表名 (字段名 字段类型...);
```

**输入**

```mysql
create table user5 (id int, name varchar(20));
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
```

##### 添加唯一约束

```mysql
alter table 数据表名 add unique(字段名);
```

**输入**

```mysql
alter tabel user5 add unique (name);
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

查询当前表格得到如下

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.02 sec)
```

进行插入操作如下：

```mysql
insert into user5 values(1, '张三');
```

返回

```mysql
Query OK, 1 row affected (0.01 sec)
```

再插入

```mysql
insert into user5 values(1, '张三');
```

报错如下

```mysql
ERROR 1062 (23000): Duplicate entry '张三' for key 'name'
```

##### modify修改字段添加唯一约束

```mysql
alter table 表格名 modify 字段名 字段类型 unique;
```

**输入**

```mysql
alter table user7 modify name varchar(20) unique;
```

> 注意：如果在表格中字段name已经出现了重复的名称，那么这个唯一约束是无法添加的！

#### 删除唯一约束

```mysql
alter table 数据表名 drop index 要删除约束的字段名;
```

首先查询当前表格的情况，返回如下

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  | UNI | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.02 sec)
```

**输入**

```
alter table user6 drop index id;
```

**返回**

```mysql
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

再次查询该表格返回如下

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  | UNI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.03 sec)
```

### 非空约束

修饰的字段不能为空

```mysql
create table 表格名 (字段名 字段类型...not null);
```

**输入**

```mysql
create table user9 (id int, name varchar(20) unique not null);
```

**返回**

```mysql
Query OK, 0 rows affected (0.04 sec)
```

**查看当前表格**

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.04 sec)
```

### 默认约束

当我们插入字段值的时候，如果没有传值，就会使用默认值

```mysql
create table 表格名 (字段名 字段类型 default 默认值)
```

**输入**

```mysql
create table user10 (
	id int,
	name varchar(20),
	age int default 18);
```

**返回**

```mysql
Query OK, 0 rows affected (0.09 sec)
```

此时使用`describe user10`查看该表格的属性如下

```mysql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | 10      |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set (0.05 sec)
```

可以看出，`age`的默认参数设置为10

执行以下插入操作，只提供`id`和`name`参数：

```mysql
insert into user10 (id, name) values(1,"ZhangSan");
```

此时查看表格具体内容为

```mysql
+------+----------+------+
| id   | name     | age  |
+------+----------+------+
|    1 | ZhangSan |   10 |
+------+----------+------+
1 row in set (0.01 sec)
```

可以看到`age`被设定为默认参数10

### 外键约束

涉及到两个表：父表和子表（主表和副表）

先创建两个表：

```mysql
create table classes(
    id int primary key, 
    name varchar(20)
);

create table students(
    id int primary key, 
    name varchar(20), 
    class_id int, 
    foreign key(class_id) references classes(id)
);
```

在`students`这个表中，最后一行`foreign key(class_id) references classes(id)`表示`students`的字段`class_id`来自于`classes`中的字段`id`，`class_id`中的值必须在`id`的值之内。

执行以下插入操作都不会报错：

```mysql
insert into classes values(1,"一班");
insert into classes values(2,"二班");
insert into calsses values(3,"三班");
insert into classes values(4,"四班");

insert into students values(1001,"ZhangSan",1);
insert into students values(1002,"LiSi", 2);
insert into students values(1003,"ZhangSan",3);
insert into students values(1004,"LiSi",4);
```

但是执行下面的插入操作会报错：

```mysql
insert into students values(1005,"ZhangSan",5);
```

> - 因为class_id中的值必须在id的范围中进行选择，5不在id的值之内，因此会报错。主表中没有的数据值在副表中是不可以使用的。
> - 主表中的记录被副表引用，是不可以被删除的。