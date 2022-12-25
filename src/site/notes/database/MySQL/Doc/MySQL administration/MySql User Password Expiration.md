---
{"author":"aming","email":"jikcheng@163.com","title":"MySql User Password Expiration","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-28 14:46","tags":"MySql User Password Expiration","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-user-password-expiration/","dgPassFrontmatter":true}
---








## 默认密码过期永不过期:
```sql
mysql> select Host,User,account_locked,password_lifetime,password_expired,password_last_changed from mysql.user;
+-----------+------------------+----------------+-------------------+------------------+-----------------------+
| Host      | User             | account_locked | password_lifetime | password_expired | password_last_changed |
+-----------+------------------+----------------+-------------------+------------------+-----------------------+
| %         | dsg              | N              |              NULL | N                | 2018-10-12 05:23:22   |
| localhost | mysql.infoschema | Y              |              NULL | N                | 2018-09-30 20:04:23   |
| localhost | mysql.session    | Y              |              NULL | N                | 2018-09-30 20:04:22   |
| localhost | mysql.sys        | Y              |              NULL | N                | 2018-09-30 20:04:23   |
| localhost | root             | N              |              NULL | N                | 2018-10-12 05:22:52   |
+-----------+------------------+----------------+-------------------+------------------+-----------------------+
```
## 使某个用户密码过期

```sql
alter user dsg@'%'  PASSWORD EXPIRE;
```

- 发现用户能登陆但是不能查看数据:

```sql
[root@mysql3 ~]# mysql -u dsg -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.0.12-commercial

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> 
```
## 重新设置密码:
```sql
alter user dsg@'%' identified by '1234';
show databases
```

> [!warning]
> 就能够看到数据库信息了.

##  设置全局用户用户过期:
```
[mysqld]
default_password_lifetime=90
```
## 密码永不过期:
```
[mysqld]
default_password_lifetime=0
```
## 设置某个用户间隔多少天密码过期
```sql
ALTER USER ‘testuser’@‘localhost' PASSWORD EXPIRE INTERVAL 30 DAY;
```
## 禁用某个用户密码过期
```sql
ALTER USER 'testuser'@'localhost' PASSWORD EXPIRE NEVER;
```
## 让某个用户使用全局的密码过期策略

```sql
ALTER USER 'testuser'@'localhost' PASSWORD EXPIRE DEFAULT;
```


