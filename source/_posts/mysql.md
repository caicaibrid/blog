---
title: Mysql 简易学习
date: 2017-04-15 15:20:33
categories: Mysql
tags:
     - Mysql
---

## mysql 连接到mysql服务器

```
mysql -u user -p password
```
## 管理MySQL的命令

> - SHOW DATABASES; 列出 MySQL 数据库管理系统的数据库列表。 
> - SHOW TABLES;显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。
> - SHOW COLUMNS FROM 数据表;显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。
> - SHOW INDEX FROM 数据表;显示数据表的详细索引信息，包括PRIMARY KEY（主键）。
> - USE 数据库名；选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库。

## MySQL 创建数据库
```
create database 数据库名;
```
## MySQL 删除数据库
```
drop database 数据库名;
```
## MySQL 创建数据表
```
crate table tablename(
  id int not null auto_increment,
  name varchar(80) not null,
  primary key(id)
);
```
## MySQL 删除数据表
```
drop table tablename;
```
## MySQL 清空数据表
```
truncate 数据库名;
```
## MySQL 插入数据
```
insert into tablename (field1, field2,...fieldN ) VALUES ( value1, value2,...valueN );
```
## MySQL where 子句
```
SELECT field1, field2,...fieldN FROM table_name1, table_name2... 
[WHERE condition1 [AND [OR]] condition2.....
```
## MySQL UPDATE 查询
```
UPDATE table_name SET field1=new-value1, field2=new-value2 [WHERE Clause]
```

> - 你可以同时更新一个或多个字段。
> - 你可以在 WHERE 子句中指定任何条件。
> - 你可以在一个单独表中同时更新数据。

## MySQL DELETE 语句
```
DELETE FROM table_name [WHERE Clause]
```
## MySQL LIKE 子句

```
SELECT field1, field2,...fieldN from table_name1, table_name2... 
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'

select name from user where name like "%cai%"
```
## MySQL UNION 操作符

```
MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。
SELECT expression1, expression2, ... expression_n 
FROM tables [WHERE conditions]
UNION [ALL | DISTINCT]

SELECT expression1, expression2, ... expression_n FROM tables [WHERE conditions];

SELECT country FROM Websites UNION SELECT country FROM apps ORDER BY country;
```
## MySQL 排序

```
SELECT field1, field2,...fieldN 
from table_name1, table_name2... 
ORDER BY field1, [field2...] [ASC [DESC]]

select * from user order by name asc
```
## MySQL GROUP BY 语句

```
SELECT column_name, function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;

mysql> select * from user;
+----+---------+------------+
| id | name    | time_count |
+----+---------+------------+
|  1 | caicai2 |          1 |
|  2 | caicai3 |          2 |
|  3 | caicai3 |          3 |
|  5 | caicai5 |          5 |
|  6 | caicai5 |          5 |
+----+---------+------------+

mysql> select name,sum(time_count) from user group by name;　name字段对应的名字出现的总次数的和
+---------+-----------------+
| name    | sum(time_count) |
+---------+-----------------+
| caicai2 |               1 |
| caicai3 |               5 |
| caicai5 |              10 |
+---------+-----------------+

mysql> select name,count(time_count) from user group by name;　name字段对应的名字出现的次数　
+---------+-------------------+
| name    | count(time_count) |
+---------+-------------------+
| caicai2 |                 1 |
| caicai3 |                 2 |
| caicai5 |                 2 |
+---------+-------------------+

```
## Mysql 连接的使用

> - INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。
> - LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
> - RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

```
mysql> SELECT * FROM tcount_tbl;
+-----------------+----------------+
| runoob_author | runoob_count |
+-----------------+----------------+
| mahran          |             20 |
| mahnaz          |           NULL |
| Jen             |           NULL |
| Gill            |             20 |
| John Poul       |              1 |
| Sanjay          |              1 |
+-----------------+----------------+
mysql> SELECT * from runoob_tbl;
+-------------+----------------+-----------------+-----------------+
| runoob_id | runoob_title | runoob_author | submission_date |
+-------------+----------------+-----------------+-----------------+
|           1 | Learn PHP      | John Poul       | 2007-05-24      |
|           2 | Learn MySQL    | Abdul S         | 2007-05-24      |
|           3 | JAVA Tutorial  | Sanjay          | 2007-05-06      |
+-------------+----------------+-----------------+-----------------+
```
### 内连接 INNER JOIN
```
mysql> SELECT a.runoob_id, a.runoob_author, b.runoob_count 
FROM runoob_tbl a 
INNER JOIN tcount_tbl b 
ON a.runoob_author = b.runoob_author;

+-----------+---------------+--------------+
| runoob_id | runoob_author | runoob_count |
+-----------+---------------+--------------+
|         1 | John Poul     |            1 |
|         3 | Sanjay        |            1 |
+-----------+---------------+--------------+
等价于
mysql> SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a, tcount_tbl b WHERE a.runoob_author = b.runoob_author;
+-------------+-----------------+----------------+
| runoob_id | runoob_author | runoob_count |
+-------------+-----------------+----------------+
|           1 | John Poul       |              1 |
|           3 | Sanjay          |              1 |
+-------------+-----------------+----------------+
```
### 左连接 LEFT JOIN
```
mysql> SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a LEFT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
+-------------+-----------------+----------------+
| runoob_id | runoob_author | runoob_count |
+-------------+-----------------+----------------+
|           1 | John Poul       |              1 |
|           2 | Abdul S         |           NULL |
|           3 | Sanjay          |              1 |
+-------------+-----------------+----------------+
LEFT JOIN，该语句会读取左边的数据表runoob_tbl的所有选取的字段数据，即便在右侧表tcount_tbl中没有对应的runoob_author字段值
```
### 右连接 RIGHT JOIN
```
mysql> SELECT b.runoob_id, b.runoob_author, a.runoob_count FROM tcount_tbl a LEFT JOIN runoob_tbl b ON a.runoob_author = b.runoob_author;
+-------------+-----------------+----------------+
| runoob_id | runoob_author | runoob_count |
+-------------+-----------------+----------------+
|           1 | John Poul       |              1 |
|           2 | Abdul S         |           NULL |
|           3 | Sanjay          |              1 |
+-------------+-----------------+----------------+
RIGHT JOIN，该语句会读取右边的数据表 runoob_tbl 的所有选取的字段数据，即便在左侧表tcount_tbl中没有对应的runoob_author字段值。
```
## MySQL NULL 值处理
> - is null 　查询包括null的数据　

```
mysql> SELECT * FROM tcount_tbl WHERE runoob_count IS NULL;
+-----------------+----------------+
| runoob_author | runoob_count |
+-----------------+----------------+
| mahnaz          |           NULL |
| Jen             |           NULL |
+-----------------+----------------+
mysql> SELECT * FROM tcount_tbl WHERE runoob_count = NULL;
Empty set (0.00 sec)
```

> - is not null 查询不包括null的数据

```
mysql> SELECT * from tcount_tbl 
       WHERE runoob_count IS NOT NULL;
+-----------------+----------------+
| runoob_author | runoob_count |
+-----------------+----------------+
| mahran          |             20 |
| Gill            |             20 |
+-----------------+----------------+
mysql> SELECT * FROM tcount_tbl WHERE runoob_count != NULL;
Empty set (0.01 sec)
```
## MySQL 正则表达式 REGEXP

```
查找name字段中以'st'为开头的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^st';
查找name字段中以'ok'为结尾的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
查找name字段中包含'mar'字符串的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'mar';
查找name字段中以元音字符开头或以'ok'字符串结尾的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';
```
## MySQL ALTER命令
### 删除，添加字段

```
mysql> ALTER TABLE testalter_tbl  DROP i; 删除表的ｉ字段
mysql> ALTER TABLE testalter_tbl ADD i INT;　添加表的ｉ字段并且为int型
```
### 修改字段类型及名称 在ALTER命令中使用 MODIFY 或 CHANGE 子句 。

```
mysql> ALTER TABLE testalter_tbl MODIFY c CHAR(10); 把字段 c 的类型从 CHAR(1) 改为 CHAR(10)

mysql> ALTER TABLE testalter_tbl CHANGE j j INT; 
使用 CHANGE 子句, 语法有很大的不同。 在 CHANGE 关键字之后，紧跟着的是你要修改的字段名，然后指定新字段名及类型。
```
### 修改表名
```
ALTER TABLE testalter_tbl RENAME TO alter_tbl; 将数据表 testalter_tbl 重命名为 alter_tbl
```
