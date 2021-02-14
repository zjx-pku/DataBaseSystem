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

### 查询students表中的所有记录的name,sex,class列

### 查询教师所有的单位（即不重复的department列）

### 查询score表中成绩在60~80之间的所有记录

### 查询score表中成绩为85,86或88的记录

### 查询students表中"95031"班性别为女的同学记录

### 以class降序查询students表的所有记录

### 以courseNum升序degree降序查询score表的所有记录

### 查询"95031"班的学生人数

### 查询score表中的最高分的学生学号和课程号（子查询或者排序）