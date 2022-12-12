---
{"author":"aming","email":"jikcheng@163.com","title":"Mysql MAC Role","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"Mysql MAC Role","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/mysql-mac-role/","dgPassFrontmatter":true}
---




## 1. 定义强制角色


通过mandatory_roles系统变量的值,可以将角色指定为强制角色.服务器将会强制角色授予所有用户的帐号,因此不需要显示的授予账户.
如果想要启动时指定强制角色请在配置文件中使用 mandatory_roles  
```
[mysqld]
mandatory_roles='role1,role2@localhost,r3@%.example.com'
```
或者:

设置自动持久化操作.

```sql
SET PERSIST mandatory_roles = 'role1,role2@localhost,r3@%.example.com';
```
设定这个值需要 `ROLE_ADMIN` 权限,并且还需要 `SYSTEM_VARIABLES_ADMIN` 或者 `SUPER` 权限.这些权限的具体内容在数据库角色章节有详细说明:

强制角色在激活之前不会生效. 如果 在登录前,激活了activate_all_roles_on_login 系统变量,则强制角色就会自动生效.或者在 登录时 使用SET ROLE 激活角色.

在强制引用后不能使用revoke 命令进行撤销角色,也不能使用DROP ROLE 或DROP USER 删除这些角色.


## 1.1. 首先建一个角色并授权：
```sql

CREATE ROLE 'app_developer';
GRANT ALL ON dsg.* TO 'app_developer';
```

## 1.2. 然后我们把这个参数设成这个角色：
```sql
SET PERSIST mandatory_roles = 'app_developer@%';
```

## 1.3. 然后我们创建一个用户 并不赋权
```sql

create user 'dsg3'@'%' identified by 'dsg3';
FLUSH PRIVILEGES;
```

## 1.4. 登录账户
我们在其它的窗口上用新加的这个帐号进去。然后可以直接set这个role

```sql
[root@dbserver ~]# mysql -u dsg3 -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
+--------------------+
1 row in set (0.09 sec)

mysql> show grants 
    -> ;
+----------------------------------+
| Grants for dsg3@%                |
+----------------------------------+
| GRANT USAGE ON *.* TO `dsg3`@`%` |
+----------------------------------+
1 row in set (0.02 sec)

mysql> 
```

```sql
set role app_developer;
Query OK, 0 rows affected (0.00 sec)
```

```sql
mysql> show variables like '%login%';
+-----------------------------+-------+
| Variable_name               | Value |
+-----------------------------+-------+
| activate_all_roles_on_login | OFF   |
+-----------------------------+-------+
1 row in set (0.28 sec)
```

::: alert-info
说明:  
activate_all_roles_on_login 参数为关闭状态否则会自动生效角色.
:::

说明:
    发现是没有权限可以看到dsg  数据库,说明强制角色现在还没生效.

## 1.5. 生效强制角色

```sql

[root@dbserver ~]# mysql -u dsg3 -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set role app_developer;
Query OK, 0 rows affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| dsg                |
| information_schema |
+--------------------+
2 rows in set (0.02 sec)

mysql> show grants
    -> ;
+-----------------------------------------------+
| Grants for dsg3@%                             |
+-----------------------------------------------+
| GRANT USAGE ON *.* TO `dsg3`@`%`              |
| GRANT ALL PRIVILEGES ON `dsg`.* TO `dsg3`@`%` |
| GRANT `app_developer`@`%` TO `dsg3`@`%`       |
+-----------------------------------------------+
3 rows in set (0.00 sec)

mysql> 
```
说明:
    获得了所有app_developer 所有的权限.
    且一直有效,直到去掉把参数中的值 .

## 1.6. 进行查询

然后如果我们把这个参数改成空的，相应的权限也就没有了。 这个不用退出会话就会生效
```sql
test2@(none) 01:17:03>set role app_developer;
ERROR 3530 (HY000): `app_developer`@`%` is not granted to `test2`@`%`
```

Note:
::: alert-info

需要注意的是 当一个角色被set了之后是不可以被删除的和revoke
```sql

mysql> drop role app_developer
    -> ;
ERROR 3628 (HY000): The role `app_developer`@`%` is a mandatory role and can't be revoked or dropped. The restriction can be lifted by excluding the role identifier from the global variable mandatory_roles.
```
:::
