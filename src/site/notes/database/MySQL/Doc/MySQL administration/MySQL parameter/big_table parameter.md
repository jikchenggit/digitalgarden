---
{"author":"aming","email":"jikcheng@163.com","title":"big_table parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"big_table parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/big-table-parameter/","dgPassFrontmatter":true}
---




# 1. big_table 参数
## 1.1. [`big_tables`]

|          Property          |      Value      |
| -------------------------- | --------------- |
| **Command-Line Format**    | `--big-tables`  |
| **System Variable**        | `[big_tables]`  |
| **Scope**                  | Global, Session |
| **Dynamic**                | Yes             |
| **[SET_VAR] Hint Applies** | No              |
| **Type**                   | boolean         |
| **Default Value**          | `OFF`           |
大表是否放在内存中,如果OFF ,则放到磁盘,如果ON 则放到内存.
但是放到内存中去后,select 语句有可能发生错误,例如table_name is full.