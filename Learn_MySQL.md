[学习视频](https://www.bilibili.com/video/BV1Vt411z7wy?p=8&spm_id_from=pageDriver)

# Learn MySQL

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

## 二、命令行工具操作数据库

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

