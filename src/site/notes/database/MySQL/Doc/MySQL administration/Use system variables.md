---
{"author":"aming","email":"jikcheng@163.com","title":"Use system variables","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 21:26","tags":"Use system variables","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/use-system-variables/","dgPassFrontmatter":true}
---





## 概述
系统维护着很多变量,这些变量表示当前服务器是怎样配置的.
每个系统变量都会有默认值.
 - 系统变量设置
 1 .可以使用启动MySQL 服务时在命令行上指定.
 2.可以在配置文件中配置.
 3. 大部分的系统变量可以使用set 语句动态改变(不必重启MySQL).
 4. 还可以使用系统变量进行表达式运算.

    服务器插件性功能的相关系统变量使用插件名称的前缀:例如log_fiter_gragnet 日志过滤组件的对应系统变量名为 log_error_filter_rules ,全名叫:dragnet.log_error_filter_rules.

### 系统变量的作用范围

1. global 全局变量影响着整个MySQL 服务.
2. session 变量只影响当前客户端连接的独立连接.

-  系统变量可以同事具有全局值和会话值.全局变量和会话变量的关系如下:

1. 当服务器启动时,全局变量都会初始化为默认值,然后通过命令或者选项文件中指定选项更改这些默认值.

2. 服务器还会维护每个连接的客户机的会话变量.会话变量使用全局变量的当前值进行初始化.

**例如**
:客户机的sql mode 有sql_mode  系统变量控制,当客户机连接时,sql_mode 值会被全局变量的sql_mode 赋值.


3. 当系统启动时指定的选项包含数字时,请指定对应的单位(单位可以大写,可以小写),如: K,M,G 

##  更改变量

### 命令行操作:
```bash
mysqld --innodb_log_file_size=16M --max_allowed_packet=1G
```
### 配置文件操作:
```
[mysqld]
innodb_log_file_size=16M
max_allowed_packet=1G
```
:::alert-warning
对于单位,大小写皆可,16m 和 16M 是同样的.
:::
### set 语句

- 对于系统变量而大部分都是动态的可以使用set 语句进行更改.修改时前面要加上修饰符.
 `global` , `@@global`关键字指出以下变量为全局的.

```bash
SET GLOBAL max_connections = 1000;
SET @@global.max_connections = 1000;
```
```bash
SET GLOBAL max_allowed_packet = 1G;
SET @@global.max_allowed_packet = 1G;
```
:::alert-info
Note:
1. 当你改变了 全局变量. 会影响MySQL 整个服务器. 受影响的是全局变量.
 如果会话重新登录后,会话变量则会变成global 变量默认值,则会话变量改变.
2. 当你改变了 会话变量,受影响的只是当前会话`show session variables`.重新登录后则恢复global 默认值.

:::

- 另个 设置全局变量的方式

```sql
SET PERSIST max_connections = 1000;
SET @@persist.max_connections = 1000;
```
- set  分为 global 和session ,请参考表2 :进行对应的参数设置.

**表2 : set  的 变量值**

|            \\             |     set     |  set global   | set session |           set        PERSIST           |         eg          |
| ------------------------- | ----------- | ------------- | ----------- | -------------------------------------- | ------------------- |
| 全局变量                  | 报错无法设置 | 只设置global级 | 报错        | 只设置global ,且写入mysql_auto.cnf 文件 | max_connections     |
| 全局变量 又为 session 变量 | 只设置会话级 | 只设置global级 | 只设置会话级 | 只设置global ,且写入mysql_auto.cnf 文件 | default_week_format |
| session 变量              | 只设置会话级 | 报错           | 只设置会话级 | 报错                                   | last_insert_id      |

- 对于全局变量必须加上global 否则会报错.
`
set max_connections = 700;
ERROR 1229 (HY000): Variable 'max_connections' is a GLOBAL variable and should be set with SET GLOBAL
eg1:
`
-  即为global又为session的变量.更改global变量值 ,客户端退出重连接后,session 值便会变为global值.

### 查看变量

查看变量使用 `show variables ` 语句.

**表1:show variables 的变量值.**

|            \\            | show variables | show  global variables | show session variables |         eg          |
| ------------------------ | -------------- | ---------------------- | ---------------------- | ------------------- |
| 全局变量                 | 显示global 值   | 显示global值            | 显示global值            | max_connections     |
| 全局变量 又为 session变量 | 显示session值   | 显示global值            | 显示session值           | default_week_format |
| session 变量             | 显示session值   | 显示为空                | 显示session 值          | last_insert_id      |
