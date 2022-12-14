---
{"author":"aming","email":"jikcheng@163.com","title":"Tutorial","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Tutorial","File Folder with relative path":"database/MySQL/Doc/MySQL Tutorial","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-tutorial/tutorial/","dgPassFrontmatter":true}
---



## 连接到数据库

```bash
mysql -h  host  u  user -p
```

- 如果连接的时候 使用本机登录

```bash
mysql -u user -p
```

> 注意：
如果MySQL 是从5.7 升级而来 。请保留默认密码加密方式。或者更改加密方式。

```sql
mysql> select user,host,plugin from user;

+------------------+-----------+-----------------------+

| user | host | plugin |

+------------------+-----------+-----------------------+

| dsg | % | caching\_sha2\_password |

| mysql.infoschema | localhost | caching\_sha2\_password |

| mysql.session | localhost | mysql\_native\_password |

| mysql.sys | localhost | caching\_sha2\_password |

| root | localhost | caching\_sha2\_password |

+------------------+-----------+-----------------------+

5 rows in set (0.00 sec)
```

```sql
mysql> ALTER USER 'dsg'@'%' IDENTIFIED WITH mysql_native_password BY '3345091';

Query OK, 0 rows affected (0.04 sec)
```

否则会报错

```log
ERROR 2061 (HY000): Authentication plugin 'caching_sha2_password' reported error: Authentication requires secure connection.
```

本机登录如果连接不上。说明mysqld 服务没有启动。

```log
 ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' 
```
### 查询版本，日期

```sql
SELECT VERSION(), CURRENT_DATE;
```

### 计算语句

```sql
SELECT SIN(PI()/4), (4+1)5;
```

### 查询格式

多条查询语句合并成单行

```
SELECT VERSION(); SELECT NOW();
```

### 退出

立即退出查询语句，当前语句不会被执行

```
mysql> ^C
```

```
mysql> select 
```

```
    -> user()
```

```
    -> ,
```

```
    -> /c
```

```
    -> \c
```

```
mysql> \c
```

## 创建和使用数据库

###  显示数据库

```
show databases;
```

### 创建数据库

```
create database test;
```

###  赋权

赋权这个数据库所有对象给某个用户

```
grant all on test. to 'dsg'@'%';
```

NOTE:

 在linux 下创建数据库的名字大小写是敏感的。例如TEST 和test 是两个不同的数据库。

###  链接到数据库

```
mysql -h 192.168.10.143 -u dsg -p3345091  test
```

> NOTE:

```
cat >> ~/.bash_profile << EOF
alias rtest='mysql -h 192.168.10.143 -u dsg -p3345091  test'
alias ms='mysql -u root -p3345091'
EOF
```

###  查看当前链接的数据

```
SELECT
```

### 创建表

 - 显示当前数据库有哪些表

```
SHOW TABLES;
```

 - 创建表

```
CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),
species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
```

 - 查看表结构

```
desc pet;
```

### 装载数据到表中

####  使用文本文件装载。
- 原理：
可以使用文本文件进行装载数据。

| name | owner | species | sex | birth | death |
|  --- | --- | --- |  --- | --- | --- |
| Fluffy | Harold | cat | f | 1993-02-04 |  
| Claws | Gwen | cat | m | 1994-03-17 |  
| Buffy | Harold | dog | f | 1989-05-13 |  
| Fang | Benny | dog | m | 1990-08-27 |  
| Bowser | Diane | dog | m | 1979-08-31 | 1995-07-29 |
| Chirpy | Gwen | bird | f | 1998-09-11 |  
| Whistler | Gwen | bird |  | 1997-12-09 |
| Slim | Benny | snake | m | 1996-04-29 |  

> 存放到pet.txt 文本之中。发现其中有空值。使用\\N 来替代

例如：

```
Whistler Gwen bird \N 1997-12-09 \N
```

##### CASE1:

-  编辑文本

```txt
Fluffy  Harold  cat     f       1993-02-04      \N
Claws   Gwen    cat     m       1994-03-17      \N
Buffy   Harold  dog     f       1989-05-13      \N
Fang    Benny   dog     m       1990-08-27      \N
Bowser  Diane   dog     m       1979-08-31      1995-07-29
Chirpy  Gwen    bird    f       1998-09-11      \N
Whistler        Gwen    bird    \N      1997-12-09      \N
Slim    Benny   snake   m       1996-04-29      \N
```

> NOTE:
 使用此文档需要打开local_infile 功能

   ` SHOW VARIABLES LIKE 'local_infile'; `
   ` set GLOBAL local_infile=1; `

-  执行装载命令


```
mysql -u root -p3345091 --local-infile=1
load data local infile '/root/pet.txt' into table pet;
```

## 查询语句




### 简单查询

 - 查询全部数据

```
SELECT  FROM pet
```

  - 查询特殊行

```
SELECT  FROM pet WHERE name = 'Bowser';
```

### 日期查询

 - 查询带日期的数据

```sql
select  from pet where birth >='1998-1-1';
select  from pet where birth >='19980101';
select  from pet where birth >='1998,1,1';
select  from pet where birth >='1998.1.1';
```

> NOTE:
 从以上看出只要日期时间格式对应年月日。和分割符没关系。

### And 语句

```sql
select  from pe species ='dog' and sex='f';
```

### OR 语句

```sql
SELECT  FROM pet WHERE species = 'snake' OR species = 'bird';
```

### AND 和OR 语句

```sql
SELECT  FROM pet WHERE (species = 'cat' AND sex = 'm')

OR (species = 'dog' AND sex = 'f');
```

###  查询指定列

```sql
SELECT name, birth FROM pet;
```

### 去重

```sql
SELECT DISTINCT owner FROM pet;
```

###  查询指定行指定列

```sql
SELECT name, species, birth FROM pet
WHERE species = 'dog' OR species = 'cat';
```

### 排序 升序

```sql
SELECT name, birth FROM pet ORDER BY birth;
```

### 排序 降序

```sql
SELECT name, birth FROM pet ORDER BY birth DESC;
```

###  排序多个字段

```sql
SELECT name, species, birth FROM pet
```

  `ORDER BY species, birth DESC;`

### 日期计算

  - 计算日期相减算年龄

```sql
select name,birth,curdate() ,timestampdiff(year,birth,curdate()) as age  from pet;
```

 - 安照name 排序，日期函数。

```sql
SELECT name, birth, CURDATE(),
TIMESTAMPDIFF(YEAR,birth,CURDATE()) AS age
FROM pet ORDER BY name;
```

 - 按照年纪排序

```sql
SELECT name, birth, CURDATE(),
TIMESTAMPDIFF(YEAR,birth,CURDATE()) AS age
FROM pet ORDER BY age;
```

###  NULL 值的使用

 - 排除已经死了的动物

```sql
SELECT name, birth, death,
TIMESTAMPDIFF(YEAR,birth,death) AS age
FROM pet WHERE death IS  NULL ORDER BY age;
```

 - 确定 死亡的动物的寿命

```sql
select name ,birth,death ,timestampdiff(year,birth,death) as age from pet
where death is not null order by age;
```

- NULL 值 计算

SELECT 1 IS NULL, 1 IS NOT NULL;

NULL 与任何值比较都为NULL

SELECT 1 IS NULL, 1 IS NOT NULL;

NULL 不等于0 也不等于 ’’

SELECT 0 IS NULL, 0 IS NOT NULL, '' IS NULL, '' IS NOT NULL;

### 日期截取

  - 截取月份

```sql
SELECT name, birth, MONTH(birth) FROM pet;
```

 - 截取年

```sql
SELECT name, birth, YEAR(birth) FROM pet;
```

 - 截取日期

```sql
SELECT name, birth, DAYOFMONTH(birth) FROM pet;
```

 - 按照日期结算

```sql
SELECT name, birth FROM pet WHERE MONTH(birth) = 5;
```

 - 表达式匹配

```sql
SELECT  FROM pet WHERE name LIKE 'b%';
SELECT  FROM pet WHERE name LIKE '%fy';
SELECT  FROM pet WHERE name LIKE '%w%';
SELECT  FROM pet WHERE name LIKE '_____';
```

### 正则表达式匹配

`.` 匹配任意单个字符

`[...]` 代表匹配三个任意字符例如abc .

`*` 匹配一个或者多个字符

`[0-9]` 代表匹配一个或者多个数字

 `^` 代表从某某字符开始。` 
`$` 是到某某结束。且忽略大小写。

- 例如：

```sql
SELECT  FROM pet WHERE REGEXP_LIKE(name, '^b');
```

 - 强制正则表达式会区分大小写。

```sql
SELECT  FROM pet WHERE REGEXP\_LIKE(name, '^b' COLLATE utf8mb4\_0900\_as\_cs);

SELECT  FROM pet WHERE REGEXP_LIKE(name, BINARY '^b');

SELECT  FROM pet WHERE REGEXP_LIKE(name, '^b', 'c');
```

 - 到某个字母结束

```sql
SELECT  FROM pet WHERE REGEXP_LIKE(name, 'fy$')
```

- 包含某个字母

```sql
SELECT  FROM pet WHERE REGEXP_LIKE(name, 'w');
```

- 匹配字符串长度

```sql
SELECT  FROM pet WHERE REGEXP_LIKE(name, '^.....$');
```

- 匹配字符长度

```sql
SELECT  FROM pet WHERE REGEXP_LIKE(name, '^.{5}$');
```

### 统计

```sql
SELECT COUNT() FROM pet;
```

- 按照某一列分别统计

```sql
SELECT owner, COUNT() FROM pet GROUP BY owner;
SELECT species, COUNT() FROM pet GROUP BY species;
```

- 按照性别统计

```sql
SELECT sex, COUNT() FROM pet GROUP BY sex;
```

 - 多列统计

```sql
SELECT species, sex, COUNT() FROM pet GROUP BY species, sex;
```

 - 添加where 条件统计

```sql
SELECT species, sex, COUNT() FROM pet

WHERE species = 'dog' OR species = 'cat'

GROUP BY species, sex;
```

- 排空统计

```sql
SELECT species, sex, COUNT() FROM pet
WHERE sex IS NOT NULL
GROUP BY species, sex;
```

> NOTE:
MYSQL 提供两种模式
 `[ONLY_FULL_GROUP_BY](file:///D:/refman-8.0-en.html-chapter/server-administration.html#sqlmode_only_full_group_by)`  模式提供通常统计函数和GROUP BY 的查询规范。

```sql
SET sql_mode = 'ONLY_FULL_GROUP_BY';
SELECT owner, COUNT() FROM pet;
```

```
ERROR 1140 (42000): In aggregated query without GROUP BY, expression
#1 of SELECT list contains nonaggregated column 'menagerie.pet.owner';
this is incompatible with sql_mode=only_full_group_by

```
 如果这个参数设置为空

```
SET sql_mode = '';
```

 - 则会强制统计不会报错

```sql
SELECT owner, COUNT() FROM pet;
```

### 使用多表关联查询

 - 创建事件表

```bash
CREATE TABLE event (name VARCHAR(20), date DATE,
type VARCHAR(15), remark VARCHAR(255));
LOAD DATA LOCAL INFILE 'event.txt' INTO TABLE event;
```

 - 关联查询JOIN ON

```sql
SELECT pet.name,
TIMESTAMPDIFF(YEAR,birth,date) AS age,
remark
FROM pet INNER JOIN event
ON pet.name = event.name;
```

###  Mysql 批量模式

 - 本地批量模式

```bash
mysql < batch-file
```

- 远程批量模式

```bash
mysql -h host -u user -p < batch-file
```

## 使用批量模式情景

- 执行重复查询（每天，每周，每月）避免重复输入输入重复命令。

- 可以编写一个新查询语句从已经存在的批量模式。

- 查询产生很多语句重定向输出到其他文件中以待备查。

```bash
mysql < batch-file | more

mysql < batch-file \> mysql.out
```

l 使用定时任务

###  交互式模式输出和批量模式输出区别

 - 交互式模式

```
+---------+
| species |
+---------+
| bird |
| cat |
| dog |
| hamster |
| snake |
+---------+
```

 - 批量模式

```
species
bird
cat
dog
hamster
snake
```

NOTE:

如果想使用交互式模式方式输出格式

 [mysql -t](file:///D:/refman-8.0-en.html-chapter/programs.html#mysql "4.5.1 mysql — The MySQL Command-Line Tool")

显示被执行的语句

[mysql -v](file:///D:/refman-8.0-en.html-chapter/programs.html#mysql "4.5.1 mysql — The MySQL Command-Line Tool").

可以使用更多方式进行批量模式输出

```bash
mysql> source filename;
mysql> \. filename
```

## 简单的查询语句

 - 创建表，装载数据。

```sql
CREATE TABLE shop (
 article INT(4) UNSIGNED ZEROFILL DEFAULT '0000' NOT NULL,
 dealer CHAR(20) DEFAULT '' NOT NULL,
 price DOUBLE(16,2) DEFAULT '0.00' NOT NULL,
 PRIMARY KEY(article, dealer));
INSERT INTO shop VALUES
 (1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),
 (3,'C',1.69),(3,'D',1.25),(4,'D',19.95);
```

```sql
SELECT  FROM shop;
```

 - 查询最大值
```sql
SELECT MAX(article) AS article FROM shop;
```
- 子查询
```sql
SELECT article, dealer, price
FROM shop
WHERE price=(SELECT MAX(price) FROM shop);
```
### 连接查询

```sql
SELECT s1.article, s1.dealer, s1.price
FROM shop s1
LEFT JOIN shop s2 ON s1.price < s2.price
WHERE s2.article IS NULL;
```
### 统计查询

```sql
SELECT article, dealer, price
FROM shop
ORDER BY price DESC
LIMIT 1;
```

### 统计最大值查询

```sql
SELECT article, MAX(price) AS price
FROM shop
GROUP BY article;
```

### 使用自定义变量

```sql
mysql> SELECT @min\_price:=MIN(price),@max\_price:=MAX(price) FROM shop;
+------------------------+------------------------+
| @min\_price:=MIN(price) | @max\_price:=MAX(price) |
+------------------------+------------------------+
| 1.25 |                 19.95 |
+------------------------+------------------------+
1 row in set (0.00 sec)
mysql> SELECT  FROM shop WHERE price=@min\_price OR price=@max\_price;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
| 0003 | D | 1.25 |
| 0004 | D | 19.95 |
+---------+--------+-------+
2 rows in set (0.00 sec)
```

### UNION

```sql
SELECT field1\_index, field2\_index
FROM test\_table WHERE field1\_index = '1'
UNION
SELECT field1\_index, field2\_index
FROM test\_table WHERE field2\_index = '1';
```

###  检查表记录发生次数

```sql
CREATE TABLE t1 (year YEAR(4), month INT(2) UNSIGNED ZEROFILL,
day INT(2) UNSIGNED ZEROFILL);
INSERT INTO t1 VALUES(2000,1,1),(2000,1,20),(2000,1,30),(2000,2,2),
(2000,2,23),(2000,2,23);
```

> NOTE:
通过表中的值，进行BIT_OR 进行亦或操作。再用BIT_COUNT 统计。如果位运算的结果一致则。当天发生了两次访问。
`
SELECT year,month,BIT\_COUNT(BIT\_OR(1<<day)) AS days FROM t1
 GROUP BY year,month;
`

### 自动增长值

```sql
CREATE TABLE animals (
 id MEDIUMINT NOT NULL AUTO_INCREMENT,
 name CHAR(30) NOT NULL,
 PRIMARY KEY (id)
);
INSERT INTO animals (name) VALUES
 ('dog'),('cat'),('penguin'),
 ('lax'),('whale'),('ostrich');
SELECT  FROM animals;
```

 - 查询结果如下

```sql
+----+---------+
| id | name |
+----+---------+
| 1 | dog |
| 2 | cat |
| 3 | penguin |
| 4 | lax |
| 5 | whale |
| 6 | ostrich |
+----+---------+
6 rows in set (0.00 sec)
```
> NOTE:
修改表的自增长值起始值。但是原来的记录值不会变。新增加的值会从100 开始。

###  Myisam 自动增长

```sql
CREATE TABLE animals (
grp ENUM('fish','mammal','bird') NOT NULL,
id MEDIUMINT NOT NULL AUTO_INCREMENT,
name CHAR(30) NOT NULL,
PRIMARY KEY (grp,id)
) ENGINE=MyISAM;
INSERT INTO animals (grp,name) VALUES
('mammal','dog'),('mammal','cat'),
('bird','penguin'),('fish','lax'),('mammal','whale'),
('bird','ostrich');
```

 - 查询结果如下

```sql
SELECT  FROM animals ORDER BY grp,id;
+--------+----+---------+
| grp | id | name |
+--------+----+---------+
| fish | 1 | lax |
| mammal | 1 | dog |
| mammal | 2 | cat |
| mammal | 3 | whale |
| bird | 1 | penguin |
| bird  | 2 | ostrich |
+--------+----+---------+
```

> NOTE:
如果自动增长列为多字段索引的一部分。就会出现重用情况。如果有`PRIMARY KEY (grp, id)` and `INDEX (id)` 两个索引，则不会出现这种情况。

## 加载Apache 日志到MySQL 中去


- 加载语句

```sql
LOAD DATA INFILE '/local/access_log' INTO TABLE tbl_name
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' ESCAPED BY '\\'
```

> NOTE:可以使用MySQL 加载对应的日志，方便用SQL查询。