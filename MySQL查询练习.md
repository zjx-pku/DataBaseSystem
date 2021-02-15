# MySQL查询练习

## 首先创建三个表

### students学生信息表

- 学号id
- 姓名name
- 性别sex
- 出生日期birthday
- 班级class

```mysql
create table students(
    id varchar(20) primary key,
    name varchar(20) not null,
    sex varchar(10) not null,
    birthday datetime,
    class varchar(20)
);
```

### teachers教师信息表

- 教师编号teacherNum
- 姓名name
- 性别sex
- 出生日期birthday
- 职称prof
- 部门department

```mysql
 create table teachers(
    id varchar(20) primary key,
    name varchar(20) not null,
    sex varchar(10) not null,
    birthday datetime,
    prof varchar(20),
    department varchar(20) not null
 );
```

### courses课程信息表

- 课程编号id
- 课程名name
- 授课教师编号teacherNum

```mysql
create table courses(
    id varchar(20) primary key,
    name varchar(20) not null,
    teacherNum varchar(20) not null,
    foreign key(teacherNum) references teachers(id)
);
```

### score成绩单

- 学号studentNum
- 课程编号courseNum
- 分数degree

```mysql
create table score(
    studentNum varchar(20) not null,
    courseNum varchar(20) not null,
    degree decimal,
    foreign key(studentNum) references students(id),
    foreign key(courseNUm) references courses(id),
    primary key(studentNum,courseNum)
);
```

## 添加数据

### 向学生信息表中添加数据

```mysql
insert into students values('101', '曾华', '男', '1977-09-01', '95033');
insert into students values('102', '匡明', '男', '1975-10-02', '95031');
insert into students values('103', '王丽', '女', '1976-01-23', '95033');
insert into students values('104', '李军', '男', '1976-02-20', '95033');
insert into students values('105', '王芳', '女', '1975-02-10', '95031');
insert into students values('106', '陆君', '男', '1974-06-03', '95031');
insert into students values('107', '王尼玛', '男', '1976-02-20', '95033');
insert into students values('108', '张全蛋', '男', '1975-02-10', '95031');
insert into students values('109', '赵铁柱', '男', '1974-06-03', '95031');
```

### 向教师信息表中添加数据

```mysql
insert into teachers values('804','李诚','男','1958-12-02', '副教授','计算机系');
insert into teachers values('856','张旭','男','1969-03-12', '讲师', '电子工程系');
insert into teachers values('825', '王萍','女','1972-05-05','助教','计算机系');
insert into teachers values('831', '刘冰', '女','1977-08-14', '助教','电子工程系');
```

### 向课程信息表中添加数据

```mysql
insert into courses values('3-105', '计算机导论', '825');
insert into courses values('3-245', '操作系统', '804');
insert into courses values('6-166', '数字电路', '856');
insert into courses values('9-888','高等数学','831');
```

### 向成绩表中添加数据

```mysql
insert into score values('103', '3-245', '86');
insert into score values('105', '3-245', '75');
insert into score values('109', '3-245', '68');
insert into score values('103', '3-105', '92');
insert into score values('105', '3-105', '88');
insert into score values('109', '3-105', '76');
insert into score values('103', '6-166', '85');
insert into score values('105', '6-166', '79');
insert into score values('109', '6-166', '81');
```

## 查询练习

### 查询students表中的所有记录

**输入**

```mysql
select * from students;
```

**返回**

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 101 | 曾华   | 男  | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女  | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男  | 1976-02-20 00:00:00 | 95033 |
| 105 | 王芳   | 女  | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 107 | 王尼玛 | 男  | 1976-02-20 00:00:00 | 95033 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
+-----+--------+-----+---------------------+-------+
9 rows in set (0.01 sec)
```



### 查询students表中的所有记录的name,sex,class列

**输入**

```mysql
select name, sex, class from students;
```

**返回**

```mysql
+-----+-----+-------+
| id  | sex | class |
+-----+-----+-------+
| 101 | 男  | 95033 |
| 102 | 男  | 95031 |
| 103 | 女  | 95033 |
| 104 | 男  | 95033 |
| 105 | 女  | 95031 |
| 106 | 男  | 95031 |
| 107 | 男  | 95033 |
| 108 | 男  | 95031 |
| 109 | 男  | 95031 |
+-----+-----+-------+
9 rows in set (0.00 sec)
```



### 查询教师所有的单位（即*不重复*的department列）

**输入**

```mysql
select distinct department from teachers;
```

**返回**

```mysql
+------------+
| department |
+------------+
| 计算机系    |
| 电子工程系  |
+------------+
2 rows in set (0.00 sec)
```

### 查询score表中成绩在60~80之间的所有记录

#### between...and...

**输入**

```mysql
select * from score where degree between 60 and 80;
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
+------------+-----------+--------+
4 rows in set (0.03 sec)
```

#### 运算符比较

**输入**

```mysql
select * from score where degree > 60 and degree < 80;
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
+------------+-----------+--------+
4 rows in set (0.01 sec)
```

### 查询score表中成绩为85,86或88的记录

**输入**

```mysql
select * from score where degree in (85,86,88);
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
3 rows in set (0.00 sec)
```

### 查询students表中"95031"班性别为女的同学记录

**输入**

```mysql
select * from students where class='95031' or sex='女';
```

**返回**

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
| 103 | 王丽   | 女  | 1976-01-23 00:00:00 | 95033 |
| 105 | 王芳   | 女  | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
+-----+--------+-----+---------------------+-------+
6 rows in set (0.01 sec)
```

### 以class降序查询students表的所有记录

**输入**

```mysql
select * from students order by class asc;
```

- ascend升序
- 不加asc和desc就是默认升序

**返回**

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女  | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
| 101 | 曾华   | 男  | 1977-09-01 00:00:00 | 95033 |
| 103 | 王丽   | 女  | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男  | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男  | 1976-02-20 00:00:00 | 95033 |
+-----+--------+-----+---------------------+-------+
```



```mysql
select * from students order by class desc;
```

- descend降序

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 101 | 曾华   | 男  | 1977-09-01 00:00:00 | 95033 |
| 103 | 王丽   | 女  | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男  | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男  | 1976-02-20 00:00:00 | 95033 |
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
| 105 | 王芳   | 女  | 1975-02-10 00:00:00 | 95031 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
+-----+--------+-----+---------------------+-------+
9 rows in set (0.00 sec)
```



### 以courseNum升序degree降序查询score表的所有记录

**输入**

```mysql
select * from score order by courseNum asc, degree desc;
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
| 103        | 3-245     |     86 |
| 105        | 3-245     |     75 |
| 109        | 3-245     |     68 |
| 103        | 6-166     |     85 |
| 109        | 6-166     |     81 |
| 105        | 6-166     |     79 |
+------------+-----------+--------+
9 rows in set (0.01 sec)
```



### 查询"95031"班的学生人数

**输入**

```mysql
select count(*) from students where class='95031';
```

**返回**

```mysql
+----------+
| count(*) |
+----------+
|        5 |
+----------+
1 row in set (0.01 sec)
```

### 查询score表中的最高分的学生学号和课程号（子查询或者排序）

#### 子查询

**输入**

```mysql
select studentNum, courseNum from score where degree=(select max(degree) from score);
```

**返回**

```mysql
+------------+-----------+
| studentNum | courseNum |
+------------+-----------+
| 103        | 3-105     |
+------------+-----------+
1 row in set (0.01 sec)
```

#### 排序

**输入**

```mysql
select studentNum, courseNum from score order by degree desc limit 0,1;
```

- limit 0,1表示从第0行开始取1行数据

**返回**

```mysql
+------------+-----------+
| studentNum | courseNum |
+------------+-----------+
| 103        | 3-105     |
+------------+-----------+
1 row in set (0.00 sec)
```

### 查询每门课的平均成绩

#### 逐门查询

**输入**

```mysql
select avg(degree) from score where courseNum='3-105';
select avg(degree) from score where courseNum='3-245';
select avg(degree) from score where courseNum='6-166';
```

**分别返回**

```mysql
+-------------+
| avg(degree) |
+-------------+
|     85.3333 |
+-------------+
1 row in set (0.00 sec)

+-------------+
| avg(degree) |
+-------------+
|     76.3333 |
+-------------+
1 row in set (0.00 sec)

+-------------+
| avg(degree) |
+-------------+
|     81.6667 |
+-------------+
1 row in set (0.00 sec)
```

#### 一条语句查询

**输入**

```mysql
select courseNum, avg(degree) from score group by courseNum;
```

**返回**

```mysql
+-----------+-------------+
| courseNum | avg(degree) |
+-----------+-------------+
| 3-105     |     85.3333 |
| 3-245     |     76.3333 |
| 6-166     |     81.6667 |
+-----------+-------------+
3 rows in set (0.00 sec)
```

### 查询score表中至少有两名学生选秀并且以3开头的课程

**输入**

```mysql
select courseNum, avg(degree) from score 
group by courseNum 
having count(courseNum)>=2 
and 
courseNum like '3%';
```

**返回**

```mysql
+-----------+-------------+
| courseNum | avg(degree) |
+-----------+-------------+
| 3-105     |     85.3333 |
| 3-245     |     76.3333 |
+-----------+-------------+
2 rows in set (0.00 sec)
```

### 查询degree在60~90之间的studentNum列

**输入**

```
select studentNum from score where degree between 60 and 90;
```

**返回**

```mysql
+------------+
| studentNum |
+------------+
| 103        |
| 103        |
| 105        |
| 105        |
| 105        |
| 109        |
| 109        |
| 109        |
+------------+
8 rows in set (0.00 sec)
```

