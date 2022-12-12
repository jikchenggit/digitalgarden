---
{"author":"aming","email":"jikcheng@163.com","title":"init_connect set User Initialize the settings","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"init_connect set User Initialize the settings","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/init-connect-set-user-initialize-the-settings/","dgPassFrontmatter":true}
---




# 1. 登陆时初始化设置


## 1.1. 此参数默认为空:
```sql
mysql> show variables like '%init_connect%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| init_connect  |       |
+---------------+-------+
1 row in set (0.12 sec)
```
## 1.2. 设置环境变量:

```sql
SET GLOBAL init_connect='SET autocommit=0';
```
## 1.3. 配置文件中配置

```
[mysqld]
init_connect='SET autocommit=0'
```

:::alert-warning
对于拥有CONNECTION_AMIND 或者 SUPER 权限,设置init_connect 参数是无效的.
是为了防止init_connect 参数设置错误而导致无法链接数据库.比如init_connect 里面有语法错误,以便有权限的用户进入数据库进行修改.
:::

## 1.4. 非super和super 用户登录区别.
### 1.4.1. 设置不自动提交
```sql
SET GLOBAL init_connect='SET autocommit=0';
```
### 1.4.2. 使用root 用户登录

```sql
[root@dbserver ~]# mysql -u root -p
Enter password: 

mysql> show variables like 'init_connect';
+---------------+------------------+
| Variable_name | Value            |
+---------------+------------------+
| init_connect  | SET autocommit=0 |
+---------------+------------------+
1 row in set (0.01 sec)

mysql> show variables like 'autocommit';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    |
+---------------+-------+
1 row in set (0.01 sec)

```
::: alert-info
发现init_connect 里面的语句并没有执行.
:::
### 1.4.3. 使用非 super 权限用户登录

```sql

[root@dbserver ~]# mysql -u dsg2 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.15 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show variables like '%autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | OFF   |
+---------------+-------+
1 row in set (0.01 sec)

```
::: alert-info
发现init_connect 语句执行了.
:::

## 1.5. 用户密码过期的登录方式
   对于密码过期的用户,init_connect 将会跳过,因为过期用户是不能执行任何语句.执行init_connect 语句将会失败,客户机无法链接.所以跳过init_connect 可以链接到数据库进行更改密码.

### 1.5.1. 正常登录

```sql

[root@dbserver ~]# mysql -u dsg2 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.15 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show variables like '%autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | OFF   |
+---------------+-------+
1 row in set (0.01 sec)
```
### 1.5.2. 强制过期dsg2 用户密码

```sql

[root@dbserver ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.15 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> alter user dsg2@'%'  PASSWORD EXPIRE;
Query OK, 0 rows affected (0.35 sec)

mysql> exit
Bye
```
### 1.5.3. 登录dsg2 用户,且更改密码.

```sql

[root@dbserver ~]# mysql -u dsg2 -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 18
Server version: 8.0.15

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER dsg2@'%' identified by '1234';
Query OK, 0 rows affected (0.28 sec)

mysql> show variables like '%autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    |
+---------------+-------+
1 row in set (0.01 sec)

mysql> 
```

:::alert-warning
发现init_connect 里的内容并没有被执行.
:::

## 1.6. 查询语句测试
服务器将会丢弃任何的查询语句返回的结果值.
