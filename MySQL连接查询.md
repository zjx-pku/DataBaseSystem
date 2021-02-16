# MySQL连接查询

## 内连接

- **内连接**：`inner join`或者`join`

## 外连接

- **左连接**：`left join`或者`left outer join`
- **右连接**：`right join`或者`right outer join`
- **完全外连接**：`full join`或者`full outer join`

## 示例

### 首先创建两个表

```mysql
create table person(
    id int,
    name varchar(20),
    cardID int
);
create table card(
    id int,
    name varchar(20)
);
```

### 插入以下数据

```mysql
insert into card values(1,'饭卡');
insert into card values(2,'建行卡');
insert into card values(3,'农行卡');
insert into card values(4,'工商卡');
insert into card values(5,'邮政卡');
insert into person values(1,'张三',1);
insert into person values(2,'李四',3);
insert into person values(3,'王五',6);
```

### 内连接查询

> 其实就是两张表中的数据，通过某个字段相等查询出相关记录数据

**输入**

```mysql
select * from person inner join card on person.cardID=card.id;
```

**返回**

```mysql
+------+------+--------+------+--------+
| id   | name | cardID | id   | name   |
+------+------+--------+------+--------+
|    1 | 张三 |      1 |    1 | 饭卡   |
|    2 | 李四 |      3 |    3 | 农行卡 |
+------+------+--------+------+--------+
2 rows in set (0.00 sec)
```

### 左连接查询

> 把左边表里面的所有数据取出来，而右边表中的数据如果符合on的条件的就显示出来，如果没有就会显示NULL

**输入**

```mysql
select * from person left join card on person.cardID=card.id;
```

**返回**

```mysql
+------+------+--------+------+--------+
| id   | name | cardID | id   | name   |
+------+------+--------+------+--------+
|    1 | 张三 |      1 |    1 | 饭卡   |
|    2 | 李四 |      3 |    3 | 农行卡 |
|    3 | 王五 |      6 | NULL | NULL   |
+------+------+--------+------+--------+
3 rows in set (0.00 sec)
```

### 右连接查询

> 把右边表里面的所有数据取出来，而左边表中的数据如果符合on的条件的就显示出来，如果没有就会显示NULL

**输入**

```mysql
select * from person right join card on person.cardID=card.id;
```

**返回**

```mysql
+------+------+--------+------+--------+
| id   | name | cardID | id   | name   |
+------+------+--------+------+--------+
|    1 | 张三 |      1 |    1 | 饭卡   |
|    2 | 李四 |      3 |    3 | 农行卡 |
| NULL | NULL |   NULL |    2 | 建行卡 |
| NULL | NULL |   NULL |    4 | 工商卡 |
| NULL | NULL |   NULL |    5 | 邮政卡 |
+------+------+--------+------+--------+
5 rows in set (0.00 sec)
```

### 全外连接查询

> mysql不支持full join，可以union左连接和右连接实现

**输入**

```mysql
select * from person left join card on person.cardID=card.id
union
select * from person right join card on person.cardID=card.id;
```

**返回**

```mysql
+------+------+--------+------+--------+
| id   | name | cardID | id   | name   |
+------+------+--------+------+--------+
|    1 | 张三 |      1 |    1 | 饭卡   |
|    2 | 李四 |      3 |    3 | 农行卡 |
|    3 | 王五 |      6 | NULL | NULL   |
| NULL | NULL |   NULL |    2 | 建行卡 |
| NULL | NULL |   NULL |    4 | 工商卡 |
| NULL | NULL |   NULL |    5 | 邮政卡 |
+------+------+--------+------+--------+
6 rows in set (0.00 sec)
```





