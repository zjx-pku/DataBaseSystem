[学习视频](https://www.bilibili.com/video/BV1Vt411z7wy?p=4&spm_id_from=pageDriver)

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

#### 登录MySQL

**输入**

```mysql
mysql -u root -p;
```

**返回**

```mysql
Enter password: 
```

#### 查询数据库服务器中所有的数据库

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

#### 选定一个数据库

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

#### 显示选定的数据库中的所有表格

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

#### 展示选定数据库的某一表格

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

#### 展示选定数据库的某一表格中的某几行

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

