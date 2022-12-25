---
{"author":"aming","email":"jikcheng@163.com","title":"File-per-table Tablespaces","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:08","tags":"File-per-table Tablespaces","File Folder with relative path":"database/MySQL/Doc/MySQL Tablespaces","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-tablespaces/file-per-table-tablespaces/","dgPassFrontmatter":true}
---





## 配置file-per-table 配置
InnoDB 默认使用file-per-table 配置,如果禁用`innodb_file_per_table` 会导致system 表空间中创建表.
## 开启file-per-table xuanxiang
```bash
[mysqld]
innodb_file_per_table=ON
```
> [!warning]
> `innodb_file_per_table` 在MySQL 5.6 以前的版本是默认启用的.
> 


# file-per-table 的优势

使用系统表空间或者通用表空间等共享表空间相比.`file-per-table` 有以下优势.
- 在截断或删除在`file-per-table` 表空间中创建的表之后，磁盘空间返回给操作系统。截断或删除存储在共享表空间中的表将在共享表空间数据文件中创建空闲空间，这些空闲空间只能用于InnoDB数据。换句话说，在表被截断或删除后，共享表空间数据文件的大小不会缩小。
- 在共享表空间中执行`ALTER TABLE` 操作会增加表空间所占用的磁盘空间.
- `file-per-table` 执行truncate 表时,性能更好.
- `file-per-table` 可以在不同的存储设备上创建,用于I/O 优化.
- `file-per-table` 方便迁移.
- 当备份 不可用是,数据损毁时,恢复的可能性会增加.
- 可以使用Mysql ENterprise backup快速备份.
# file-per-table 劣势
- 每个文件一个表空间,可能会造成同一个表使用.会导致空间浪费.
- fsync 的操作IOPS 会更高.
- mysqld 打开额文件句柄会更多.会影响性能.
- 每个表都需要打开一个文件,可能需要更多的文件描述符.
- 会有更多的碎片.会影响`DROP TABLE` 和表扫描.
- innodb_autoextend_increment 只负责共享表空间的增长控制.这个参数不会控制`file-per-table` 类型的表空间.