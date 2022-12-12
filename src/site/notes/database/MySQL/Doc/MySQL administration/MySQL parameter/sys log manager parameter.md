---
{"author":"aming","email":"jikcheng@163.com","title":"sys log manager parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 13:37","tags":"sys log manager parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/sys-log-manager-parameter/","dgPassFrontmatter":true}
---




# 1. 系统错误日志管理参数
## 1.1. [`lc_messages`]

|          Property          |        Value         |
| -------------------------- | -------------------- |
| **Command-Line Format**    | `--lc-messages=name` |
| **System Variable**        | `[lc_messages]`      |
| **Scope**                  | Global, Session      |
| **Dynamic**                | Yes                  |
| **[SET_VAR] Hint Applies** | No                   |
| **Type**                   | string               |
| **Default Value**          | `en_US`              |

设置错误消息的语言.
## 1.2. [`log_error`]

|          Property          |           Value           |
| -------------------------- | ------------------------- |
| **Command-Line Format**    | `--log-error[=file_name]` |
| **System Variable**        | `[log_error]`             |
| **Scope**                  | Global                    |
| **Dynamic**                | No                        |
| **[SET_VAR] Hint Applies** | No                        |
| **Type**                   | file name                 |



## 1.3. [`log_error_filter_rules`]

|          Property          |                                        Value                                         |
| -------------------------- | ------------------------------------------------------------------------------------ |
| **Command-Line Format**    | `--log-error-filter-rules`                                                           |
| **Introduced**             | 8.0.2                                                                                |
| **Removed**                | 8.0.4                                                                                |
| **System Variable**        | `[log_error_filter_rules](server-administration.html#sysvar_log_error_filter_rules)` |
| **Scope**                  | Global                                                                               |
| **Dynamic**                | Yes                                                                                  |
| **[SET_VAR] Hint Applies** | No                                                                                   |
| **Type**                   | string                                                                               |
| **Default Value**          | `set by server`                                                                      |

自定义时日志过滤器. 


## 1.4. [`log_error_services`]
    
|          Property          |                  Value                   |
| -------------------------- | ---------------------------------------- |
| **Command-Line Format**    | `--log-error-services`                   |
| **Introduced**             | 8.0.2                                    |
| **System Variable**        | `[log_error_services]`                   |
| **Scope**                  | Global                                   |
| **Dynamic**                | Yes                                      |
| **[SET_VAR] Hint Applies** | No                                       |
| **Type**                   | string                                   |
| **Default Value**          | `log_filter_internal; log_sink_internal` |

启用日志过滤的和写入器插件的参数.

详情请参考:

    
## 1.5. [`log_error_verbosity`]
    
| Property | Value |
| --- | --- |
| **Command-Line Format** | `--log-error-verbosity=#` |
| **System Variable** | `[log_error_verbosity](server-administration.html#sysvar_log_error_verbosity)` |
| **Scope** | Global |
| **Dynamic** | Yes |
| **[SET_VAR](optimization.html#optimizer-hints "8.9.2 Optimizer Hints") Hint Applies** | No |
| **Type** | integer |
| **Default Value** (>= 8.0.4) | `2` |
| **Default Value** (<= 8.0.3) | `3` |
| **Minimum Value** | `1` |
| **Maximum Value** | `3` |

日志记录过滤选项级别.

详细表如下.

| Desired Log Filtering             | log\_error\_verbosity Value |
| --------------------------------- | --------------------------- |
| Error messages                    | 1                           |
| Error and warning messages        | 2                           |
| Error, warning, and note messages | 3                           |




## 1.6. [`log_timestamps`]

|          Property          |        Value         |
| -------------------------- | -------------------- |
| **Command-Line Format**    | `--log-timestamps=#` |
| **System Variable**        | `[log_timestamps]`   |
| **Scope**                  | Global               |
| **Dynamic**                | Yes                  |
| **[SET_VAR] Hint Applies** | No                   |
| **Type**                   | enumeration          |
| **Default Value**          | `UTC`                |
| **Valid Values**           | `UTC``SYSTEM`        |

控制写入错误消息日志和慢速查询日志消息的时间,使用CONVERT_TZ()  可以设置所需的任何时区.


# 2. 数据库操作日志
## 2.1. [`general_log`]

|          Property          |      Value      |
| -------------------------- | --------------- |
| **Command-Line Format**    | `--general-log` |
| **System Variable**        | `[general_log]` |
| **Scope**                  | Global          |
| **Dynamic**                | Yes             |
| **[SET_VAR] Hint Applies** | No              |
| **Type**                   | boolean         |
| **Default Value**          | `OFF`           |
开启 general log 将所有到达MySQL Server的SQL语句记录下来。由于产生的日志较大,所以一般不会开启此参数.
相关参数:
general_log_file 文件保存位置.
log_output 输出类型 TABLE ,file,如果不设置这个参数,则general log 不会生效.

## 2.2. [`general_log_file`]

|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **Command-Line Format**    | `--general-log-file=file_name` |
| **System Variable**        | `[general_log_file]`           |
| **Scope**                  | Global                         |
| **Dynamic**                | Yes                            |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | file name                      |
| **Default Value**          | `host_name.log`                |

 默认值为host_name.log

The name of the general query log file. The default value is ``*`host_name`*.log``, but the initial value can be changed with the [`--general_log_file`](server-administration.html#sysvar_general_log_file) option.    

```sql


mysql> show variables like 'general_log_file';
+------------------+-----------------------------+
| Variable_name    | Value                       |
+------------------+-----------------------------+
| general_log_file | /var/lib/mysql/dbserver.log |
+------------------+-----------------------------+
1 row in set (0.01 sec)
```
## 2.3. [`log_output`]

|          Property          |                            Value                             |
| -------------------------- | ------------------------------------------------------------ |
| **Command-Line Format**    | `--log-output=name`                                          |
| **System Variable**        | `[log_output](server-administration.html#sysvar_log_output)` |
| **Scope**                  | Global                                                       |
| **Dynamic**                | Yes                                                          |
| **[SET_VAR] Hint Applies** | No                                                           |
| **Type**                   | set                                                          |
| **Default Value**          | `FILE`                                                       |
| **Valid Values**           | `TABLE``FILE``NONE`                                          |

log_output 输出记录的sql 存储类型 TABLE ,file.默认值为file.如果设置为NONE,即使启用了日志,也不会写入日志.