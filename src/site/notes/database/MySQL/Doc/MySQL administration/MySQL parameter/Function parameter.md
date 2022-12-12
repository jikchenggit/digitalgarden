---
{"author":"aming","email":"jikcheng@163.com","title":"Function parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Function parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/function-parameter/","dgPassFrontmatter":true}
---





## 函数功能
### [`cte_max_recursion_depth`]

|          Property          |             Value             |
| -------------------------- | ----------------------------- |
| **Command-Line Format**    | `--cte-max-recursion-depth=#` |
| **Introduced**             | 8.0.3                         |
| **System Variable**        | `[cte_max_recursion_depth]`   |
| **Scope**                  | Global, Session               |
| **Dynamic**                | Yes                           |
| **[SET_VAR] Hint Applies** | No                            |
| **Type**                   | integer                       |
| **Default Value**          | `1000`                        |
| **Minimum Value**          | `0`                           |
| **Maximum Value**          | `4294967295`                  |
cte_max_recursion_depth 控制调用递归的次数，默认1000次
#### 递归函数深度测试
##### 创建递归语句
```sql
WITH RECURSIVE cte (n) AS
(
SELECT 1
UNION ALL
SELECT n + 1 FROM cte WHERE n < 5
)
SELECT * FROM cte;
```
##### 字符长度:
```sql
WITH RECURSIVE cte AS
 (
   SELECT 1 AS n, CAST('abc' AS CHAR(20)) AS str
   UNION ALL
   SELECT n + 1, CONCAT(str, str) FROM cte WHERE n < 3
 )
SELECT * FROM cte;
```
#####  超过一千次调用:
```sql
WITH RECURSIVE cte (n) AS
(
SELECT 1
SELECT n + 1 FROM cte where n<1001
)
SELECT * FROM cte;
```
### [`default_week_format`]

|          Property          |           Value           |
| -------------------------- | ------------------------- |
| **Command-Line Format**    | `--default-week-format=#` |
| **System Variable**        | `[default_week_format]`   |
| **Scope**                  | Global, Session           |
| **Dynamic**                | Yes                       |
| **[SET_VAR] Hint Applies** | No                        |
| **Type**                   | integer                   |
| **Default Value**          | `0`                       |
| **Minimum Value**          | `0`                       |
| **Maximum Value**          | `7`                       |
影响week 函数:
#### week函数测试:
##### week 函数使用:
WEEK(date, mode);

1. WEEK函数接受两个参数：

date是要获取周数的日期。mode是一个可选参数，用于确定周数计算的逻辑。它允许您指定本周是从星期一还是星期日开始，返回的周数应在0到52之间或0到53之间。

| 模式 | 一周的一天 | 范围 |
| ---- | --------- | ---- |
| 0    | 星期日     | 0-53 |
| 1    | 星期一     | 0-53 |
| 2    | 星期二     | 1-53 |
| 3    | 星期三     | 1-53 |
| 4    | 星期四     | 0-53 |
| 5    | 星期五     | 0-53 |
| 6    | 星期六     | 1-53 |
| 7    | 星期一     | 1-53 |


##### 查询语句:
```sql
use yiibaidb;SELECT 
    WEEK(orderDate) week_no, 
    COUNT(*)
FROM
    orders
WHERE
    YEAR(orderDate) = 2013
GROUP BY WEEK(orderDate);
Database change
```
##### 设置星期为1 开始:
```sql
mysql> show variables like 'default_week_format';
+---------------------+-------+
| Variable_name       | Value |
+---------------------+-------+
| default_week_format | 1     |
+---------------------+-------+
1 row in set (0.00 sec)
```


### [`div_precision_increment`]

|          Property          |             Value             |
| -------------------------- | ----------------------------- |
| **Command-Line Format**    | `--div-precision-increment=#` |
| **System Variable**        | `[div_precision_increment]`   |
| **Scope**                  | Global, Session               |
| **Dynamic**                | Yes                           |
| **[SET_VAR] Hint Applies** | Yes                           |
| **Type**                   | integer                       |
| **Default Value**          | `4`                           |
| **Minimum Value**          | `0`                           |
| **Maximum Value**          | `30`                          |

计算分数小数精度.
```sql
mysql> sELECT 1/7;
+--------+
| 1/7    |
+--------+
| 0.1429 |
+--------+
```
默认4位小数.
 更改精度

```sql
mysql>  SET div_precision_increment = 12;
Query OK, 0 rows affected (0.00 sec)

mysql> sELECT 1/7;
+----------------+
| 1/7            |
+----------------+
| 0.142857142857 |
+----------------+
1 row in set (0.00 sec)
```

### [`lc_time_names`]

|          Property          |       Value       |
| -------------------------- | ----------------- |
| **System Variable**        | `[lc_time_names]` |
| **Scope**                  | Global, Session   |
| **Dynamic**                | Yes               |
| **[SET_VAR] Hint Applies** | No                |
| **Type**                   | string            |
 - 设置时间的地区,这个设置影响DATE_FORMAT()、DAYNAME()和MONTHNAME() 的输出结果.

## 3. 调度功能
### 3.1. [`event_scheduler`]

|                                       Property                                        |            Value            |
| ------------------------------------------------------------------------------------- | --------------------------- |
| **Command-Line Format**                                                               | `--event-scheduler[=value]` |
| **System Variable**                                                                   | `[event_scheduler]`         |
| **Scope**                                                                             | Global                      |
| **Dynamic**                                                                           | Yes                         |
| **[SET_VAR](optimization.html#optimizer-hints "8.9.2 Optimizer Hints") Hint Applies** | No                          |
| **Type**                                                                              | enumeration                 |
| **Default Value** (>= 8.0.3)                                                          | `ON`                        |
| **Default Value** (<= 8.0.2)                                                          | `OFF`                       |
| **Valid Values**                                                                      | `ON``OFF``DISABLED`         |

### 3.2. [`group_concat_max_len`]

|               Property               |           Value            |
| ------------------------------------ | -------------------------- |
| **Command-Line Format**              | `--group-concat-max-len=#` |
| **System Variable**                  | `[group_concat_max_len]`   |
| **Scope**                            | Global, Session            |
| **Dynamic**                          | Yes                        |
| **[SET_VAR]Hint Applies**            | Yes                        |
| **Type** (64-bit platforms)          | integer                    |
| **Type** (32-bit platforms)          | integer                    |
| **Default Value** (64-bit platforms) | `1024`                     |
| **Default Value** (32-bit platforms) | `1024`                     |
| **Minimum Value** (64-bit platforms) | `4`                        |
| **Minimum Value** (32-bit platforms) | `4`                        |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`     |
| **Maximum Value** (32-bit platforms) | `4294967295`               |

MySQL提供的group_concat函数可以拼接某个字段值成字符串，如 select group_concat(user_name) from sys_user,默认的分隔符是 逗号，即"," ，如果需要自定义分隔符可以使用 SEPARATOR

如：select group_concat(user_name SEPARATOR '_')  from sys_user

但是如果 user_name  拼接的字符串的长度字节超过1024 则会被截断。
- 测试:
```sql
mysql> select group_concat(body  SEPARATOR '_')  from  article;                                           
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| group_concat(body  SEPARATOR '_')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 也可以在创建索引的时候指定索引的长度_创建索引的时候指定索引的长度_索引的长度_长度_大圣_齐天大圣_齐天大圣,孙悟空_齐天大圣孙悟空_齐天大圣啊!_齐天大圣,大圣_齐天大,圣_good 孙悟空_hello 孙悟空_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_hello_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_good_也可以在创建索引的时候指定索引的长度_创建索引的时候指定索引的长度_索引的长度_长度_大圣_齐天大圣_齐天大圣,孙悟空_齐天大圣孙悟空_齐天大圣啊!_齐天大圣,大圣_齐天大,圣_good                                                                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set, 1 warning (0.01 sec)

mysql> 

```

MySQL的调度开关.

## 4. 数据类型功能
### 4.1. [`explicit_defaults_for_timestamp`]

|           Property           |                 Value                 |
| ---------------------------- | ------------------------------------- |
| **Command-Line Format**      | `--explicit-defaults-for-timestamp=#` |
| **Deprecated**               | Yes                                   |
| **System Variable**          | `[explicit_defaults_for_timestamp]`   |
| **Scope**                    | Global, Session                       |
| **Dynamic**                  | Yes                                   |
| **[SET_VAR] Hint Applies**   | No                                    |
| **Type**                     | boolean                               |
| **Default Value** (>= 8.0.2) | `ON`                                  |
| **Default Value** (<= 8.0.1) | `OFF`                                 |
- 此变量确定TIMESTAMP 列中的默认值是否为空,是否采用非标准形式.
默认值是为ON,禁用非标准形式.
如果关闭此OFF,则会报错.
如果explicit_defaults_for_timestamp 是禁用的状态.则导致当前系统采用非标准形式处理
 - case1:TIMESTAMP 列如果没有显式声明为NULL 属性,则自动使用NOT NULL 属性声明.指定TIMESTAMP 字段为NULL 属性是在此模式下允许的操作.
 - case2:当TIMESTAMP 列没有显式的声明为NULL 属性,DEFAULT ,ON UPDATE  属性时,则会自动声明DEFAULT CURRENT_TIMESTAMP,ON UPDATE CURRENT_TIMESTAMP ,属性.
- 如果参数enabled,将会禁用非标准形式.

case1 :指定TIMESTMAP 类型为NULL,则无法设置当前的时间值 ,请设置为CURRENT_TIMESTAMP,或者NOW().

case2:TIMESTAMP 列不会显示的声明为NOT NUL ,

如果explicit_defaults_for_timestamp 在配置文件中禁用,则在服务启动时汇报以下错误:


::: alert-info
TIMESTAMP with implicit DEFAULT value is deprecated.
Please use --explicit_defaults_for_timestamp server option (see
documentation for more details).
:::








## 4.2. have_geometry
是否支持空间数据类型.

## 5. 用户功能
### 5.1. [`external_user`]

|          Property          |       Value       |
| -------------------------- | ----------------- |
| **System Variable**        | `[external_user]` |
| **Scope**                  | Session           |
| **Dynamic**                | No                |
| **[SET_VAR] Hint Applies** | No                |
| **Type**                   | string            |

外部认证的代理用户变量,默认为NULL




## 6. 约束功能
### 6.1. [`foreign_key_checks`]

|          Property          |          Value          |
| -------------------------- | ----------------------- |
| **System Variable**        | `[foreign_key_checks])` |
| **Scope**                  | Global, Session         |
| **Dynamic**                | Yes                     |
| **[SET_VAR] Hint Applies** | Yes                     |
| **Type**                   | boolean                 |
| **Default Value**          | `1`                     |

如果设置为1 ,外键约束对于innodb 表示强制有效的.如果设置为0 外键约束则会被忽略,但是有几个例外:
1. 当重建表删除表时,如果表的定义不符合引用表的外键约束,则会返回错误.
2. alter table 操作违反了外键约束,则也会报错.

通常我们都是使用默认打开约束检查的.但是如果禁用外键检查是为了重载表(innodb),则非常有用,因为会忽略外键的检查约束的父子关系.


设置foreign_key_checks 为0 ,也会影响到drop 操作:
1. 可以删除一个用户,这个用户中有引用其他用户的外键也可以删除掉.
2. 可以删除有外键关联的父子表.



```ad-note
设置foreign_key_checks=1 不会触发表扫描,因此可以追加rows 到表中.
设置foreign_key_checks=0 不会验证约束有效性.
设置foreign_key_checks=1 时,删除一个外键所需要的索引时,是不被允许的.
设置foreign_key_checks=0 时,要删除索引,必须先要删除外键约束.
```
# 7. 表达式功能
## 7.1. [`ft_boolean_syntax`]

|          Property          |           Value            |
| -------------------------- | -------------------------- |
| **Command-Line Format**    | `--ft-boolean-syntax=name` |
| **System Variable**        | `[ft_boolean_syntax]`      |
| **Scope**                  | Global                     |
| **Dynamic**                | Yes                        |
| **[SET_VAR] Hint Applies** | No                         |
| **Type**                   | string                     |
| **Default Value**          | `+ -><()~*:""&|`           |

此参数设置布尔表达式,用于全文搜索支持的操作符号

默认值为 `'+ -><()~*:""&|'`. 
如果改变此布尔表达式意义:必须遵守以下规范

- 运算符的意义是由字符串的位置决定
    
-  参数必须要14 个字符

- 每个字符必须是ASCII 非字母数字字符.

- 第一个或第二个字符必须是空格
   
-  除了 11 ,12 位置运算符外,其他字符不允许重复,这两个字符不需要相同,但是只能这两个位置是相同的字符.

- 位置 10,13,14 (`:`, `&`, and `|` ) 将会保留给将来的扩展.


# 8. 压缩功能
## 8.1. [`have_compress`]
 COMPRESS() and UNCOMPRESS() 这两个函数是否启用.

# 9. 插件功能
## 9.1. have_dynamic_loading
是否支持动态加载插件.如果为NO ,能动态加载,如果为OFF 则不能动态加载.
# 10. ssl 功能
## 10.1. have_ssl
是否支持SSL 链接.
# 11. 调优功能
## 11.1. have_profiling
单条语句剖析功能是否支持.
启用单条语句剖析:
 set profiling=1; -- 当前回话生效.
详情请见:
    本地:

 [单条语句剖析功能设置.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据库调优/单条语句剖析功能设置.md)
     网页:
     [http://aming.ddns.net:8900/#!MySQL/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%B0%83%E4%BC%98/单条语句剖析功能设置.md](http://aming.ddns.net:8900/#!MySQL/数据库调优/单条语句剖析功能设置.md)

# 12. 软链接功能
## 12.1. [`have_symlink`]
   mysqld支持符号链接则为YES，否则为NO。在Unix主机上，此功能对数据目录和索引目录有用。
::: alert-info
Note
这个参数将在未来的MySQL 版本中废弃.
:::
# 13. 自增序列功能
## 13.1. [`insert_id`]

配置当前会话的自增序列值.
### 13.1.1. 配置当前会话的自增序列值.

- 创建表

```sql

   CREATE TABLE article ( 
                  id INT AUTO_INCREMENT NOT NULL PRIMARY KEY, 
                  title VARCHAR(200), 
                  body TEXT, 
                  FULLTEXT(title, body) 
              );
```

- 设置自增序列为40 

```sql

mysql> set session insert_id=40;
Query OK, 0 rows affected (0.00 sec)
```

- 查看当前表中数据

```sql

mysql> select * from article;
+----+-------+-------+
| id | title | body  |
+----+-------+-------+
| 31 | serch | hello |
| 32 | serch | hello |
+----+-------+-------+
2 rows in set (0.00 sec)
```
- 插入新值

```sql

mysql> insert into dsg.article (title,body) values('serch','hello');
Query OK, 1 row affected (0.17 sec)

mysql> select * from article;
+----+-------+-------+
| id | title | body  |
+----+-------+-------+
| 31 | serch | hello |
| 32 | serch | hello |
| 40 | serch | hello |
+----+-------+-------+
3 rows in set (0.00 sec)
```
:::alert-warning

自增序从32 跳转到40 ,设置insert_id 会导致id 序号不连续.
:::

# 14. 启动参数管理
## 14.1. [`init_file`]

|                                       Property                                        |                           Value                            |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Command-Line Format**                                                               | `--init-file=file_name`                                    |
| **System Variable**                                                                   | `[init_file](server-administration.html#sysvar_init_file)` |
| **Scope**                                                                             | Global                                                     |
| **Dynamic**                                                                           | No                                                         |
| **[SET_VAR](optimization.html#optimizer-hints "8.9.2 Optimizer Hints") Hint Applies** | No                                                         |
| **Type**                                                                              | file name                                                  |

 此参数可以指定一个文件.此文件中包含MySQL 服务器启动时需要执行的命令.命令必须是一行,并且不包含注释.

# 15. 内存参数管理

## 15.1. [`large_pages`]

|          Property          |      Value      |
| -------------------------- | --------------- |
| **Command-Line Format**    | `--large-pages` |
| **System Variable**        | `[large_pages]` |
| **Scope**                  | Global          |
| **Dynamic**                | No              |
| **[SET_VAR] Hint Applies** | No              |
| **Platform Specific**      | Linux           |
| **Type** (Linux)           | boolean         |
| **Default Value** (Linux)  | `FALSE`         |
是否开启大页内存.

详情请见:

本地:
[开启大页内存.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据库调优/开启大页内存.md)

网页:
[开启大页内存.md](http://aming.ddns.net:8900/#!MySQL/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%B0%83%E4%BC%98/%E5%BC%80%E5%90%AF%E5%A4%A7%E9%A1%B5%E5%86%85%E5%AD%98.md)

## 15.2. [`arge_page_size`]

|          Property          |        Value        |
| -------------------------- | ------------------- |
| **System Variable**        | `[large_page_size]` |
| **Scope**                  | Global              |
| **Dynamic**                | No                  |
| **[SET_VAR] Hint Applies** | No                  |
| **Type** (Linux)           | integer             |
| **Default Value** (Linux)  | `0`                 |

 如果大页内存开启,这里显示的就是内存页的大小.大页内存仅在Linux 上支持,其他平台这个参数一直为零.
## 15.3. [`locked_in_memory`]

|          Property          |        Value         |
| -------------------------- | -------------------- |
| **System Variable**        | `[locked_in_memory]` |
| **Scope**                  | Global               |
| **Dynamic**                | No                   |
| **[SET_VAR] Hint Applies** | No                   |

MySQL 启动时 是否一直被锁定于内存.

# 16. 导入参数
## 16.1. [`local_infile`]

|           Property           |      Value       |
| ---------------------------- | ---------------- |
| **System Variable**          | `[local_infile]` |
| **Scope**                    | Global           |
| **Dynamic**                  | Yes              |
| **[SET_VAR] Hint Applies**   | No               |
| **Type**                     | boolean          |
| **Default Value** (>= 8.0.2) | `OFF`            |
| **Default Value** (<= 8.0.1) | `ON`             |

 - 此变量控制用于加载数据语句的服务器端本地功能。根据local_infile设置，服务器拒绝或允许在客户端启用本地的客户端加载本地数据。
# 17. 证书参数
## 17.1. [`license`]

|          Property          |    Value    |
| -------------------------- | ----------- |
| **System Variable**        | `[license]` |
| **Scope**                  | Global      |
| **Dynamic**                | No          |
| **[SET_VAR] Hint Applies** | No          |
| **Type**                   | string      |
| **Default Value**          | `GPL`       |
证书类型

```sql
mysql> select @@license;
+-----------+
| @@license |
+-----------+
| GPL       |
+-----------+
1 row in set (0.00 sec)
```
# 18. 角色参数
## 18.1. [`mandatory_roles`]

|          Property          |           Value           |
| -------------------------- | ------------------------- |
| **Command-Line Format**    | `--mandatory-roles=value` |
| **Introduced**             | 8.0.2                     |
| **System Variable**        | `[mandatory_roles]`       |
| **Scope**                  | Global                    |
| **Dynamic**                | Yes                       |
| **[SET_VAR] Hint Applies** | No                        |
| **Type**                   | string                    |
| **Default Value**          | `empty string`            |



该参数可以指定强制性的角色作为mandatory_roles系统变量的值。服务器将一个强制性的角色授予所有用户,所以它不需要明确授予任何帐户。

设置
```sql
SET PERSIST mandatory_roles = '`role1`@`%`,`role2`,role3,role4@localhost';
```

设定这个值需要 `ROLE_ADMIN` 权限,并且还需要 `SYSTEM_VARIABLES_ADMIN` 或者 `SUPER` 权限.这些权限的具体内容在数据库角色章节有详细说明:
[角色.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据库权限/角色.md)



如果设定值中有用户名和主机名,则必须用引用字符串 方式编写(也是加上单引号).

在强制引用后不能使用revoke 命令进行撤销角色,也不能使用DROP ROLE 或DROP USER 删除这些角色.



强制角色在激活之前不会生效. 如果 在登录前,激活了activate_all_roles_on_login 系统变量,则强制角色就会自动生效.或者在 登录时 使用SET ROLE 激活角色.