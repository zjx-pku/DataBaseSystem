# MySQL事务

> MySQL中，事物是一个最小的不可分割的工作单元，事物能够保证一个业务的完整性

## 类比银行转账

a取走了100元：

`update user set money=money-100 where name='a';`

b收入了100元

`update user set money=money+100 where name='b';`

在实际程序中可能会出现一条语句执行成功，而另一条没有执行成功，会造成数据前后不一致的现象。有时多条sql语句可能会有要么同时成功要么同时失败的要求。

## MySQL如何控制事务

MySQL是默认开启事务的：

**输入**

```mysql
select @@autocommit;
```

**返回**

```mysql
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
```

> 默认事务开启的作用是什么？当我们去执行一个sql语句的时候，效果会立即体现出来且不会回滚。

### 进行如下操作：创建一个数据库、添加一个数据表、插入一行数据

```mysql
create database rbtest;
create table test(
    id int primary key,
    name varchar(20),
    money int
);
insert into test values(1,'ZhangSan',100);
```

### 展示当前表格如下

```mysql
+----+----------+-------+
| id | name     | money |
+----+----------+-------+
|  1 | ZhangSan |   100 |
+----+----------+-------+
1 row in set (0.00 sec)
```

### 回滚

**输入**

```mysql
rollback;
```

**返回**

```mysql
Query OK, 0 rows affected (0.00 sec)
```

**说明当前表格没有成功回滚，查询后表格和上面一致**

> 原因是MySQL默认事务开启，我们不能进行回滚操作

### 设置默认事务关闭（自动提交关闭）

```mysql
set @@autocommit=0;;
```

### 再进行插入操作，尝试是否可以回滚

**输入**

```
insert into test values(2,'LiSi',200);
insert into test values(3,'WangWu',500);
select * from test;
rollback;
select * from test;
```

**返回**

```mysql
+----+----------+-------+
| id | name     | money |
+----+----------+-------+
|  1 | ZhangSan |   100 |
|  2 | LiSi     |   200 |
|  3 | WangWu   |   500 |
+----+----------+-------+
3 rows in set (0.00 sec)


+----+----------+-------+
| id | name     | money |
+----+----------+-------+
|  1 | ZhangSan |   100 |
+----+----------+-------+
1 row in set (0.00 sec)
```

说明两个插入操作都没有生效，为了在自动提交关闭的情况下成功使操作生效，需要在一段操作之后添加commit语句：

**输入**

```mysql
insert into test values(2,'LiSi',200);
insert into test values(3,'WangWu',500);
commit;
select * from test;
rollback;
select * from test;
```

**返回**

```mysql
+----+----------+-------+
| id | name     | money |
+----+----------+-------+
|  1 | ZhangSan |   100 |
|  2 | LiSi     |   200 |
|  3 | WangWu   |   500 |
+----+----------+-------+
3 rows in set (0.00 sec)

+----+----------+-------+
| id | name     | money |
+----+----------+-------+
|  1 | ZhangSan |   100 |
|  2 | LiSi     |   200 |
|  3 | WangWu   |   500 |
+----+----------+-------+
3 rows in set (0.00 sec)
```

## 回到银行转账

### 创建一个账单

```mysql
create table money(id int, name varchar(20), money int);
```

### 设置两人原始存款

```mysql
insert into money values(1,'a',1000);
insert into money values(2,'b',2000);
```

### 提交上述操作

```mysql
commit;
```

### 查看两人当前余额

```mysql
+------+------+-------+
| id   | name | money |
+------+------+-------+
|    1 | a    |  1000 |
|    2 | b    |  2000 |
+------+------+-------+
2 rows in set (0.00 sec)
```

### 进行转账操作

```mysql
update money set money=money-100 where name='a';
update money set money=money+100 where name='b';
```

### 查询两人当前余额

```mysql
+------+------+-------+
| id   | name | money |
+------+------+-------+
|    1 | a    |   900 |
|    2 | b    |  2100 |
+------+------+-------+
2 rows in set (0.00 sec)
```

### 发现转账操作错误，进行回滚

```mysql
rollback;
```

### 然后查询两人当前余额

```mysql
+------+------+-------+
| id   | name | money |
+------+------+-------+
|    1 | a    |  1000 |
|    2 | b    |  2000 |
+------+------+-------+
2 rows in set (0.00 sec)
```

## 在@@autocommit=1的情况下为某段代码开启事务

> 设置@@autocommit=0的话有个缺点，每段代码敲完后都要手动commit，但是有时候只需要为某段关键代码开启事务。

```mysql
begin;
...
commit;
```

或者

```mysql
start transcacion;
...
commit;
```

## 事务四大特征

- A：原子性（事务是最小的单位，不可以再分割）
- C：一致性（事务要求，同一事务中的sql语句，必须保证同时成功或同时失败）
- I：隔离性（事务1和事务2之间是具有隔离性的）
- D：持久性（事务一旦结束，就不可以返回）

## 事务操作总结

### 事务开启方式

- `set @@autocommit=0;`
- `begin...commit`
- `start transcation...commit`

### 事务手动提交

``commit`

### 事务手动回滚

`rollback`

## 事务的隔离性

### 查看隔离级别

MySQL8.0

```mysql
select @@global.transaction_isolation;
```

MySQL5.x

**输入**

```mysql
select @@global.tx_isolation;
```

**返回**

```mysql
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| REPEATABLE-READ       |
+-----------------------+
1 row in set (0.00 sec)
```

### 修改隔离级别

**输入**

```mysql
set global transaction isolation level read uncommitted;
```

**返回**

```mysql
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| READ-UNCOMMITTED      |
+-----------------------+
1 row in set (0.00 sec)
```

### 隔离级别

#### `read uncommitted`：读未提交的

> 如果有事务a，事务b：a事务对数据进行操作，在操作过程中，事务没有被提交，但是b可以看见a操作的结果

##### 脏读

如果两个不同的地方都在进行操作，如果事务a开启之后（READ-UNCOMMITTED），他它未提交的数据可以被其他事务读取到，这样就会出现**脏读**。

#### `read committed`：读已经提交的

##### 不可重复读现象

虽然我只能读到另外一个事务提交的数据，但还是会出现问题，就是读取同一个表的数据发现前后不一致（在两个操作之间别人修改并提交了数据），这就是不可重复读现象。

#### `repeatable read`：可以重复读

##### 幻读

事务a操作和事务b同时操作一张表，事务a提交的数据也不能被事务b读到，就可能造成幻读

#### `serializable`：串行化

> 当一个表被另一个事务操作的时候，其他事务里面的写操作是 不可以进行的，而是进入排队状态，（在没有等待超时的前提下）直到操作事务结束后，这个写操作才会执行。

串行化的问题是：性能特差

**隔离级别越高性能越差**：性能`read uncommitted` > `read committed` > `repeatable read` > `serializable`

**MySQL默认隔离级别是：**`repeatable read`

# 其他笔记链接

🔗https://github.com/hjzCy/sql_node/blob/master/mysql/MySQL%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md

🔗https://github.com/XiangLinPro/IT_book#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E7%9B%B8%E5%85%B3%E4%B9%A6%E7%B1%8D