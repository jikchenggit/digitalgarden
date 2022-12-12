---
{"author":"aming","email":"jikcheng@163.com","title":"Case sensitive","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Case sensitive","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/case-sensitive/","dgPassFrontmatter":true}
---




## 表参数
###  [`lower_case_table_names`]

|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **Command-Line Format**    | `--lower-case-table-names[=#]` |
| **System Variable**        | `[lower_case_table_names]`     |
| **Scope**                  | Global                         |
| **Dynamic**                | No                             |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | integer                        |
| **Default Value**          | `0`                            |
| **Minimum Value**          | `0`                            |
| **Maximum Value**          | `2`                            |

如果设置为0 ,则按照指定方式存储表明,区分大小写.
如果设置为1 ,表明将以小写存储在磁盘上,不区分大小写.
如果设置为2 ,按照给定值存储表明,使用小写字母进行比较.

在window上默认为1 ,macos 默认为2 .

在windows  或者 macOS 上 这个值不应该设置为0  ,如果设置为0 后,运行insert into select 则会损坏索引.

如果在不区分大小写的文件系统上 设置为0 ,则服务器会打印错误消息,服务器将会推出.

如果使用的InnoDB 表,应该在所有平台上该变量设置为1. 


禁止这个值 和初始化时 MySQL 数据库的初始值不一样. 因为数据字典的字段使用排序是基于初始化MySQL 数据库设定的.如果设置成不同值,则标识符和排序和比较方式的则会不同.
