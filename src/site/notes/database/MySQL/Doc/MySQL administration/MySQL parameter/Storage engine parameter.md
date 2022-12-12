---
{"author":"aming","email":"jikcheng@163.com","title":"Storage engine pamater","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Storage engine pamater","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/storage-engine-parameter/","dgPassFrontmatter":true}
---





##  数据引擎
###  [`default_storage_engine`]

|          Property          |              Value              |
| -------------------------- | ------------------------------- |
| **Command-Line Format**    | `--default-storage-engine=name` |
| **System Variable**        | `[default_storage_engine]`      |
| **Scope**                  | Global, Session                 |
| **Dynamic**                | Yes                             |
| **[SET_VAR] Hint Applies** | No                              |
| **Type**                   | enumeration                     |
| **Default Value**          | `InnoDB`                        |
默认的数据存储引擎

### [`default_tmp_storage_engine`]

|          Property          |                Value                |
| -------------------------- | ----------------------------------- |
| **Command-Line Format**    | `--default-tmp-storage-engine=name` |
| **System Variable**        | `[default_tmp_storage_engine]`      |
| **Scope**                  | Global, Session                     |
| **Dynamic**                | Yes                                 |
| **[SET_VAR] Hint Applies** | Yes                                 |
| **Type**                   | enumeration                         |
| **Default Value**          | `InnoDB`                            |

默认的temp 表引擎.

###  [`disabled_storage_engines`]

|          Property          |                      Value                      |
| -------------------------- | ----------------------------------------------- |
| **Command-Line Format**    | `--disabled-storage-engines=engine[,engine]...` |
| **System Variable**        | `[disabled_storage_engines]`                    |
| **Scope**                  | Global                                          |
| **Dynamic**                | No                                              |
| **[SET_VAR] Hint Applies** | No                                              |
| **Type**                   | string                                          |
| **Default Value**          | `empty string`                                  |


禁用的存储引擎
例子:
```
[mysqld]
disabled_storage_engines="MyISAM,FEDERATED"
```
##  临时表引擎

### [`internal_tmp_disk_storage_engine`]

|          Property          |                 Value                  |
| -------------------------- | -------------------------------------- |
| **Command-Line Format**    | `--internal-tmp-disk-storage-engine=#` |
| **System Variable**        | `[internal_tmp_disk_storage_engine])`  |
| **Scope**                  | Global                                 |
| **Dynamic**                | Yes                                    |
| **[SET_VAR] Hint Applies** | No                                     |
| **Type**                   | enumeration                            |
| **Default Value**          | `INNODB`                               |
| **Valid Values**           | `MYISAM``INNODB`                       |



```ad-note
说明:临时表的存储引擎.
```

磁盘上内存临时表的存储引擎.默认为MyISAM 和 INNODB.

对于优化器使用的也是此参数:internl_tmp_disk_storage_engine. 所定义的临时表定义的存储引擎.

如果使用默认值INNODB 时,查询临时表的函数或者列数超过了存储引擎的限制.那么将存储引擎设置为myisam.


### [`internal_tmp_mem_storage_engine`]

|          Property          |                 Value                 |
| -------------------------- | ------------------------------------- |
| **Command-Line Format**    | `--internal-tmp-mem-storage-engine=#` |
| **Introduced**             | 8.0.2                                 |
| **System Variable**        | `[internal_tmp_mem_storage_engine]`   |
| **Scope**                  | Global, Session                       |
| **Dynamic**                | Yes                                   |
| **[SET_VAR] Hint Applies** | Yes                                   |
| **Type**                   | enumeration                           |
| **Default Value**          | `TempTable`                           |
| **Valid Values**           | `TempTable``MEMORY`                   |




```ad-note
说明:临时表的存储引擎.
```

临时表使用内存或者TempTale 存储引擎.

优化器也使用此参数设置的存储引擎.