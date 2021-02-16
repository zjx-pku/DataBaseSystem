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
    teacherNum varchar(20) primary key,
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

### 查询所有学生的name, courseNum, degree列

**输入**

```mysql
select name, courseNum, degree from students, score where students.id=score.studentNum;
```

**返回**

```mysql
+--------+-----------+--------+
| name   | courseNum | degree |
+--------+-----------+--------+
| 王丽   | 3-105     |     92 |
| 王丽   | 3-245     |     86 |
| 王丽   | 6-166     |     85 |
| 王芳   | 3-105     |     88 |
| 王芳   | 3-245     |     75 |
| 王芳   | 6-166     |     79 |
| 赵铁柱 | 3-105     |     76 |
| 赵铁柱 | 3-245     |     68 |
| 赵铁柱 | 6-166     |     81 |
+--------+-----------+--------+
9 rows in set (0.01 sec)
```

### 查询所有学生的id, name(course), degree列

**输入**

```mysql
select students.id, courses.name, degree from students, score, courses where students.id=score.studentNum and score.courseNum=courses.id;
```

**返回**

```mysql
+-----+------------+--------+
| id  | name       | degree |
+-----+------------+--------+
| 103 | 计算机导论 |     92 |
| 105 | 计算机导论 |     88 |
| 109 | 计算机导论 |     76 |
| 103 | 操作系统   |     86 |
| 105 | 操作系统   |     75 |
| 109 | 操作系统   |     68 |
| 103 | 数字电路   |     85 |
| 105 | 数字电路   |     79 |
| 109 | 数字电路   |     81 |
+-----+------------+--------+
9 rows in set (0.03 sec)
```

### 查询所有学生的students.name, courses.name, degree列

**输入**

```mysql
select students.name as studentName, courses.name as courseName, degree from students, courses, score where students.id=score.studentNum and score.courseNum=courses.id;
```

**返回**

```mysql
+-------------+------------+--------+
| studentName | courseName | degree |
+-------------+------------+--------+
| 王丽        | 计算机导论 |     92 |
| 王芳        | 计算机导论 |     88 |
| 赵铁柱      | 计算机导论 |     76 |
| 王丽        | 操作系统   |     86 |
| 王芳        | 操作系统   |     75 |
| 赵铁柱      | 操作系统   |     68 |
| 王丽        | 数字电路   |     85 |
| 王芳        | 数字电路   |     79 |
| 赵铁柱      | 数字电路   |     81 |
+-------------+------------+--------+
9 rows in set (0.00 sec)
```

### 查询"95031"班学生每门课的平均分

**输入**

```
select courseNum, avg(degree) from score where studentNum in (select id from students where class='95031') group by courseNum;
```

**返回**

```mysql
+-----------+-------------+
| courseNum | avg(degree) |
+-----------+-------------+
| 3-105     |     82.0000 |
| 3-245     |     71.5000 |
| 6-166     |     80.0000 |
+-----------+-------------+
3 rows in set (0.01 sec)
```

### 查询选修"3-105"课程的成绩高于"109"号同学"3-105"成绩的所有同学的记录

**输入**

```mysql
select * from score where courseNum='3-105' and degree > (select degree from score where studentNum='109' and courseNum='3-105');
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
2 rows in set (0.00 sec)
```

### 查询成绩高于学号为"109"课程号为"3-105"的成绩的所有记录

**输入**

```mysql
select * from score where degree>(select degree from score where studentNum='109' and courseNum='3-105');
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
6 rows in set (0.00 sec)
```

### 查询和学号为108,101的同学同年出生的所有学生的id,name,birthday

**输入**

```
select id,name,birthday from students where year(birthday) in (select year(birthday) from students where id in ('108', '101'));
```

**返回**

```mysql
+-----+--------+---------------------+
| id  | name   | birthday            |
+-----+--------+---------------------+
| 101 | 曾华   | 1977-09-01 00:00:00 |
| 102 | 匡明   | 1975-10-02 00:00:00 |
| 105 | 王芳   | 1975-02-10 00:00:00 |
| 108 | 张全蛋 | 1975-02-10 00:00:00 |
+-----+--------+---------------------+
4 rows in set (0.00 sec)
```

### 查询"张旭"老师任课的学生成绩

**输入**

```mysql
select * from score where courseNum in (select id from courses where teacherNum=(select teacherNum from teachers where name='张旭'));
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 6-166     |     85 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
3 rows in set (0.00 sec)
```

### 查询选修某课程的同学人数多于2人的教师姓名

**输入**

```mysql
select name from teachers where teacherNum in (select teacherNum from score where courseNum in (select courseNum from score group by courseNum having count(*)>2));
```

**返回**

```mysql
+------+
| name |
+------+
| 李诚 |
| 王萍 |
| 刘冰 |
| 张旭 |
+------+
4 rows in set (0.00 sec)
```

### 查询"95033"班和"95031"班全体学生的记录

**输入**

```mysql
select * from students where class in ("95033","95031");
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
9 rows in set (0.00 sec)
```

### 查询存在85分以上成绩的课程id

**输入**

```mysql
select courseNum ,degree from score where degree > 85;
```

**返回**

```mysql
+-----------+--------+
| courseNum | degree |
+-----------+--------+
| 3-105     |     92 |
| 3-245     |     86 |
| 3-105     |     88 |
+-----------+--------+
3 rows in set (0.00 sec)
```

### 查询出"计算机系"教师所教课程的成绩表

**输入**

```mysql
select * from score where courseNum in (select id from courses where teacherNum in (select teacherNum from teachers where department='计算机系'));
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-245     |     86 |
| 105        | 3-245     |     75 |
| 109        | 3-245     |     68 |
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
6 rows in set (0.00 sec)
```

### 查询"计算机系"与"电子工程系"不同职称教师的name和prof

**输入**

```mysql
select * from teachers where department="计算机系" and prof not in (select prof from teachers where department="电子工程系")
union
select * from teachers where department="电子工程系" and prof not in (select prof from teachers where department="计算机系");
```

**返回**

```mysql
+------------+------+-----+---------------------+--------+------------+
| teacherNum | name | sex | birthday            | prof   | department |
+------------+------+-----+---------------------+--------+------------+
| 804        | 李诚 | 男  | 1958-12-02 00:00:00 | 副教授 | 计算机系   |
| 856        | 张旭 | 男  | 1969-03-12 00:00:00 | 讲师   | 电子工程系 |
+------------+------+-----+---------------------+--------+------------+
2 rows in set (0.00 sec)
```

### 查询选修编号为"3-105"课程且成绩至少高于选修编号为"3-245"同学的courseNum, studentNum, degree,并按degree从高到底排序

**输入**

#### 方法一：使用min

```
select * from score where courseNum='3-105' and degree > (select min(degree) from score where courseNum='3-245') order by degree desc;
```

#### 方法二：使用any

```mysql
select * from score where courseNum='3-105' and degree > any(select degree from score where courseNum='3-245') order by degree desc;
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
3 rows in set (0.00 sec)
```

### 查询选修编号为"3-105"课程且成绩所有高于选修编号为"3-245"同学的courseNum, studentNum, degree,并按degree从高到底排序

**输入**

#### 方法一：使用min

```mysql
select * from score where courseNum='3-105' and degree > (select max(degree) from score where courseNum='3-245') order by degree desc;
```

#### 方法二：使用all

```mysql
select * from score where courseNum='3-105' and degree > all(select degree from score where courseNum='3-245') order by degree desc;
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
2 rows in set (0.00 sec)
```

### 查询所有教师和同学的name,sex,birthday

**输入**

```
select name, sex, birthday from teachers
union
select name, sex, birthday from students;
```

**返回**

```mysql
+--------+-----+---------------------+
| name   | sex | birthday            |
+--------+-----+---------------------+
| 李诚   | 男  | 1958-12-02 00:00:00 |
| 王萍   | 女  | 1972-05-05 00:00:00 |
| 刘冰   | 女  | 1977-08-14 00:00:00 |
| 张旭   | 男  | 1969-03-12 00:00:00 |
| 曾华   | 男  | 1977-09-01 00:00:00 |
| 匡明   | 男  | 1975-10-02 00:00:00 |
| 王丽   | 女  | 1976-01-23 00:00:00 |
| 李军   | 男  | 1976-02-20 00:00:00 |
| 王芳   | 女  | 1975-02-10 00:00:00 |
| 陆君   | 男  | 1974-06-03 00:00:00 |
| 王尼玛 | 男  | 1976-02-20 00:00:00 |
| 张全蛋 | 男  | 1975-02-10 00:00:00 |
| 赵铁柱 | 男  | 1974-06-03 00:00:00 |
+--------+-----+---------------------+
13 rows in set (0.00 sec)
```

### 查询所有"女"教师和"女"同学的name, sex, birthday

**输入**

```mysql
select name, sex, birthday from teachers where sex="女"
union
select name, sex, birthday from students where sex="女";
```

**返回**

```mysql
+------+-----+---------------------+
| name | sex | birthday            |
+------+-----+---------------------+
| 王萍 | 女  | 1972-05-05 00:00:00 |
| 刘冰 | 女  | 1977-08-14 00:00:00 |
| 王丽 | 女  | 1976-01-23 00:00:00 |
| 王芳 | 女  | 1975-02-10 00:00:00 |
+------+-----+---------------------+
4 rows in set (0.00 sec)
```

### 查询成绩比该课程平均成绩低的同学的成绩表

**输入**

```mysql
select * from score a where degree<(select avg(degree) from score b where a.courseNum = b.courseNum);
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
| 109        | 6-166     |     81 |
+------------+-----------+--------+
5 rows in set (0.00 sec)
```

### 查询所有任课教师的name,department

**输入**

```mysql
select name, department from teachers where teacherNum in (select teacherNum from courses);
```

**返回**

```mysql
+------+------------+
| name | department |
+------+------------+
| 李诚 | 计算机系   |
| 王萍 | 计算机系   |
| 刘冰 | 电子工程系 |
| 张旭 | 电子工程系 |
+------+------------+
4 rows in set (0.00 sec)
```

### 查询至少有2名男生的编号

**输入**

```mysql
select class from students group by class having count(sex="男")>1;
```

**返回**

```mysql
+-------+
| class |
+-------+
| 95031 |
| 95033 |
+-------+
2 rows in set (0.00 sec)
```

### 查询student表中不姓王的同学记录

**输入**

```mysql
select * from students where name not like '王%';
```

**返回**

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 101 | 曾华   | 男  | 1977-09-01 00:00:00 | 95033 |
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
| 104 | 李军   | 男  | 1976-02-20 00:00:00 | 95033 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
+-----+--------+-----+---------------------+-------+
6 rows in set (0.00 sec)
```

### 查询student表中每个学生的姓名和年龄

**输入**

```mysql
select name, year(now())-year(birthday) as '年龄' from students;
```

**返回**

```mysql
+--------+------+
| name   | 年龄 |
+--------+------+
| 曾华   |   44 |
| 匡明   |   46 |
| 王丽   |   45 |
| 李军   |   45 |
| 王芳   |   46 |
| 陆君   |   47 |
| 王尼玛 |   45 |
| 张全蛋 |   46 |
| 赵铁柱 |   47 |
+--------+------+
9 rows in set (0.00 sec)
```

### 查询student表中最大和最小的birthday日期值

**输入**

```mysql
select max(birthday) as max_birthday, min(birthday) as min_birthday from students;
```

**返回**

```mysql
+---------------------+---------------------+
| max_birthday        | min_birthday        |
+---------------------+---------------------+
| 1977-09-01 00:00:00 | 1974-06-03 00:00:00 |
+---------------------+---------------------+
1 row in set (0.00 sec)
```

### 以班号和年龄从大到小的顺序查询students表中的全部记录

**输入**

```mysql
select * from students order by class desc, birthday;
```

**返回**

```mysql
+-----+--------+-----+---------------------+-------+
| id  | name   | sex | birthday            | class |
+-----+--------+-----+---------------------+-------+
| 103 | 王丽   | 女  | 1976-01-23 00:00:00 | 95033 |
| 104 | 李军   | 男  | 1976-02-20 00:00:00 | 95033 |
| 107 | 王尼玛 | 男  | 1976-02-20 00:00:00 | 95033 |
| 101 | 曾华   | 男  | 1977-09-01 00:00:00 | 95033 |
| 106 | 陆君   | 男  | 1974-06-03 00:00:00 | 95031 |
| 109 | 赵铁柱 | 男  | 1974-06-03 00:00:00 | 95031 |
| 105 | 王芳   | 女  | 1975-02-10 00:00:00 | 95031 |
| 108 | 张全蛋 | 男  | 1975-02-10 00:00:00 | 95031 |
| 102 | 匡明   | 男  | 1975-10-02 00:00:00 | 95031 |
+-----+--------+-----+---------------------+-------+
9 rows in set (0.00 sec)
```

### 查询男教师及其所上课程

**输入**

```mysql
select teachers.name, courses.name from courses, teachers where courses.teacherNum in (select teacherNum from teachers where sex='男') and courses.teacherNum=teachers.teacherNum;
```

**输出**

```mysql
+------+----------+
| name | name     |
+------+----------+
| 李诚 | 操作系统 |
| 张旭 | 数字电路 |
+------+----------+
2 rows in set (0.00 sec)
```

### 查询最高分同学的studentNum, courseNum, degree列

**输入**

```mysql
select studentNum, courseNum, degree from score where degree = (select max(degree) from score);
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
+------------+-----------+--------+
1 row in set (0.00 sec)
```

### 查询和“李军”性别相同的所有同学的name

**输入**

```mysql
select name from students where sex=(select sex from students where name='李军');
```

**返回**

```mysql
+--------+
| name   |
+--------+
| 曾华   |
| 匡明   |
| 李军   |
| 陆君   |
| 王尼玛 |
| 张全蛋 |
| 赵铁柱 |
+--------+
7 rows in set (0.00 sec)
```

### 查询和李军同性别并且同班的同学name

**输入**

```mysql
select name from students where sex = (select sex from students where name='李军') and class = (select class from students where name='李军');
```

**返回**

```mysql
+--------+
| name   |
+--------+
| 曾华   |
| 李军   |
| 王尼玛 |
+--------+
3 rows in set (0.00 sec)
```

### 查询所有选修计算机导论课程的男同学的成绩表

**输入**

```mysql
select * from score where score.courseNum = (select courses.id from courses where courses.name = '计算机导论') and score.studentNum in (select students.id from students where students.sex = '男');
```

**返回**

```mysql
+------------+-----------+--------+
| studentNum | courseNum | degree |
+------------+-----------+--------+
| 109        | 3-105     |     76 |
+------------+-----------+--------+
1 row in set (0.00 sec)
```

### 查询所有同学的studentNum, courseNum, grade列

假设使用下面的命令创建了 一个grade表：

```mysql
create table grade values(low int(3), upp int(3), grade char(1));
```

插入数据如下：

```mysql
insert into grade values(90,100,'A');
insert into grade values(80,89,'B');
insert into grade values(70,79,'C');
insert into grade values(60,69,'D');
 insert into grade values(0,59,'E');
```

**输入**

```mysql
select studentNum, courseNum, grade from score, grade where degree between low and upp;
```

**返回**

```mysql
+------------+-----------+-------+
| studentNum | courseNum | grade |
+------------+-----------+-------+
| 103        | 3-105     | A     |
| 103        | 3-245     | B     |
| 103        | 6-166     | B     |
| 105        | 3-105     | B     |
| 105        | 3-245     | C     |
| 105        | 6-166     | C     |
| 109        | 3-105     | C     |
| 109        | 3-245     | D     |
| 109        | 6-166     | B     |
+------------+-----------+-------+
9 rows in set (0.00 sec)
```

