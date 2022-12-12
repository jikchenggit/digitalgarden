---
{"author":"aming","email":"jikcheng@163.com","title":"TIMSTMAP Data_type","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 21:08","tags":"TIMSTMAP Data_type","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/timstmap-data-type/","dgPassFrontmatter":true}
---






# 2. TIMSTMAP 类型规则测试
## 2.1. 测试代码:

```sql
mysql -u root -p
drop database dsg;
create database dsg;
use dsg
drop table time1;
drop table time2;
set global explicit_defaults_for_timestamp=1;
set session explicit_defaults_for_timestamp=1;
create table time1 (id INT,time_test timestamp);
insert into time1(id) values(1);
insert into time1(id) values(2);
insert into time1(id) values(3);
select * from time1;
desc time1;
set global explicit_defaults_for_timestamp=0;
set session explicit_defaults_for_timestamp=0;
create table time2 (id INT,time_test timestamp);
insert into time2(id) values(1);
insert into time2(id) values(2);
insert into time2(id) values(3);
select * from time2;
desc time2;
```


```sql
mysql> set global explicit_defaults_for_timestamp=1;
Query OK, 0 rows affected (0.00 sec)

mysql> set session explicit_defaults_for_timestamp=1;
Query OK, 0 rows affected (0.00 sec)

mysql> create table time1 (id INT,time_test timestamp);
insert into time1(id) values(2);
insert into time1(id) values(3);
select * from time1;
desc time1;




Query OK, 0 rows affected (1.31 sec)

mysql> insert into time1(id) values(1);
Query OK, 1 row affected (0.20 sec)

mysql> insert into time1(id) values(2);
Query OK, 1 row affected (0.19 sec)

mysql> insert into time1(id) values(3);

Query OK, 1 row affected (0.19 sec)

mysql> select * from time1;
+------+-----------+
| id   | time_test |
+------+-----------+
|    1 | NULL      |
|    2 | NULL      |
|    3 | NULL      |
+------+-----------+
3 rows in set (0.00 sec)

mysql> desc time1;
+-----------+-----------+------+-----+---------+-------+
| Field     | Type      | Null | Key | Default | Extra |
+-----------+-----------+------+-----+---------+-------+
| id        | int(11)   | YES  |     | NULL    |       |
| time_test | timestamp | YES  |     | NULL    |       |
+-----------+-----------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> 
mysql> 
mysql> 
mysql> 
mysql> 
mysql> 
mysql> set global explicit_defaults_for_timestamp=0;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> set session explicit_defaults_for_timestamp=0;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> create table time2 (id INT,time_test timestamp);
insert into time2(id) values(1);
insert into time2(id) values(2);
insert into time2(id) values(3);
select * from time2;
desc time2;


Query OK, 0 rows affected (1.03 sec)

mysql> insert into time2(id) values(1);

Query OK, 1 row affected (0.18 sec)

mysql> insert into time2(id) values(2);

Query OK, 1 row affected (0.26 sec)

mysql> insert into time2(id) values(3);

Query OK, 1 row affected (0.20 sec)

mysql> select * from time2;
+------+---------------------+
| id   | time_test           |
+------+---------------------+
|    1 | 2019-04-03 14:55:49 |
|    2 | 2019-04-03 14:55:49 |
|    3 | 2019-04-03 14:55:49 |
+------+---------------------+
3 rows in set (0.00 sec)

mysql> desc time2;
+-----------+-----------+------+-----+-------------------+-----------------------------------------------+
| Field     | Type      | Null | Key | Default           | Extra                                         |
+-----------+-----------+------+-----+-------------------+-----------------------------------------------+
| id        | int(11)   | YES  |     | NULL              |                                               |
| time_test | timestamp | NO   |     | CURRENT_TIMESTAMP | DEFAULT_GENERATED on update CURRENT_TIMESTAMP |
+-----------+-----------+------+-----+-------------------+-----------------------------------------------+
2 rows in set (0.00 sec)
```
