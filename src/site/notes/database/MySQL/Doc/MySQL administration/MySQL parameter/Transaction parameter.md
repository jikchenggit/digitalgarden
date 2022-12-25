---
{"author":"aming","email":"jikcheng@163.com","title":"Transaction parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Transaction parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/transaction-parameter/","dgPassFrontmatter":true}
---





## 提交参数
###  [`autocommit`]

|          Property          |       Value        |
| -------------------------- | ------------------ |
| **Command-Line Format**    | `--autocommit[=#]` |
| **System Variable**        | `[autocommit]`     |
| **Scope**                  | Global, Session    |
| **Dynamic**                | Yes                |
| **[SET_VAR] Hint Applies** | No                 |
| **Type**                   | boolean            |
| **Default Value**          | `ON`               |

自动提交:
case1: 如果自动提交打开,则每次更新数据都会自动提交,例如exit,commit,session 断开,clent 报错断开.
case2: 如果自动提交关闭,则每次数据都需要显式提交,例如:commit,或者rollback.如果客户端正常退出,或者异常退出,则所有数据都不会被提交.
自动提交选项为:
```
[mysqld]
autocommit=0
```
## 事务独立参数
### [`completion_type`]

|                                       Property                                        |                                 Value                                  |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Command-Line Format**                                                               | `--completion-type=#`                                                  |
| **System Variable**                                                                   | `[completion_type] |
| **Scope**                                                                             | Global, Session                                                        |
| **Dynamic**                                                                           | Yes                                                                    |
| **[SET_VAR]int Applies** | No                                                                     |
| **Type**                                                                              | enumeration                                                            |
| **Default Value**                                                                     | `NO_CHAIN`                                                             |
| **Valid Values**                                                                      | `NO_CHAIN``CHAIN``RELEASE``0``1``2`                                    |
事物回话独立


 - commit和commit work级别一致，都用来提交事务。(rollback和rollback work跟commit与commit work的关系一样)不同之处在于commit work用来控制事务结束后的行为是chain还是release的。
- 如果是chain方式，那么就变成了链事务。用户可以通过参数completion_type来控制。默认为0，表示没有任何操作，此时commit=commit work


```sql
root@localhost:employees 05:24:36> show variables like '%comple%';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| completion_type | NO_CHAIN |
+-----------------+----------+
1 row in set (0.00 sec)
```




> [!note]
> 当参数completion_type的值为1时，commit work等同于commit and chain，表示马上自动开启一个相同隔离级别的事务


## 超时参数
[`lock_wait_timeout`]

|          Property          |          Value          |
| -------------------------- | ----------------------- |
| **Command-Line Format**    | `--lock-wait-timeout=#` |
| **System Variable**        | `[lock_wait_timeout]`   |
| **Scope**                  | Global, Session         |
| **Dynamic**                | Yes                     |
| **[SET_VAR] Hint Applies** | Yes                     |
| **Type**                   | integer                 |
| **Default Value**          | `31536000`              |
| **Minimum Value**          | `1`                     |
| **Maximum Value**          | `31536000`              |

获取元数据锁的超时时限,最小值为1秒.最大值为1年.默认是一年.

此超时适用于所有使用元数据锁的语句。这些操作包括表、视图、存储过程和存储函数上的DML和DDL操作，以及锁表、用读锁刷新表和处理程序语句。

此超时不适用于系统表的隐式访问,例如:grant 和revoke .但是超时可以适用与select 和update 表.

当超过这个值后,则会报错:
Error: 1205 SQLSTATE: HY000 (ER_LOCK_WAIT_TIMEOUT)

如果使用了LOCK INSTANCE FOR BACKUP  进行备份.此参数对这个操作也是适用的.