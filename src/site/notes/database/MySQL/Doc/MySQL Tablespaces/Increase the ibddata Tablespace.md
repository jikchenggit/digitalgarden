---
{"author":"aming","email":"jikcheng@163.com","title":"Increase the ibddata Tablespace","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 21:22","tags":"Increase the ibddata Tablespace","File Folder with relative path":"database/MySQL/Doc/MySQL Tablespaces","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-tablespaces/increase-the-ibddata-tablespace/","dgPassFrontmatter":true}
---






## 增大系统表空间

system表空间是InnoDB数据字典、doublewrite缓冲区、change缓冲区和undo日志的存储区域。如果表是在system表空间中创建的，而不是在file-per-table或常规表空间中创建的，那么它还可能包含表和索引数据。

```bash
innodb_data_home_dir =
innodb_data_file_path = /ibdata/ibdata1:10M:autoextend
```

```bash
innodb_data_home_dir =
innodb_data_file_path = /ibdata/ibdata1:988M;/disk2/ibdata2:50M:autoextend
```

> [!warning]
> 注意增大系统表空间不能使用现有的表空间,否则会报错.
## 减小InnoDB 系统表空间大小
1. 使用`mysqldump` 备份MySQL 模式下的所有innodb 表
```bash
SELECT TABLE_NAME from INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA='mysql' and ENGINE='InnoDB';
```
2. 停止MySQL
3. 删除所有现有的表空间文件(*.ibd)，包括ibdata和ib_log文件。不要忘记删除*。位于mysql模式中的表的ibd文件。
4. 删除所有innoDB  表的.frm 文件.
5. 配置新的system 表空间.
6. 重启数据库.
7. 导入数据.
> [!note]
> 为了更好的发挥性能可以直接使用裸设备.
