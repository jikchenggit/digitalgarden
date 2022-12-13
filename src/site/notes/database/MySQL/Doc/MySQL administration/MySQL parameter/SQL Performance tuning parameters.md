---
{"author":"aming","email":"jikcheng@163.com","title":"SQL Performance tuning parameters","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 21:27","tags":"SQL Performance tuning parameters","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/sql-performance-tuning-parameters/","dgPassFrontmatter":true}
---






# 1. sql 性能参数



# 2. 索引调优
## 2.1. [`eq_range_index_dive_limit`]

|          Property          |             Value             |
| -------------------------- | ----------------------------- |
| **System Variable**        | `[eq_range_index_dive_limit]` |
| **Scope**                  | Global, Session               |
| **Dynamic**                | Yes                           |
| **[SET_VAR] Hint Applies** | Yes                                                                                        |
| **Type**                   | integer                       |
| **Default Value**          | `200`                         |
| **Minimum Value**          | `0`                           |
| **Maximum Value**          | `4294967295`                  |
在使用IN或者OR等条件进行查询时，MySQL使用eq_range_index_dive_limit参数来判断使用index dive还是使用index statistics方式来进行预估：
1、当低于eq_range_index_dive_limit参数阀值时，采用index dive方式预估影响行数，该方式优点是相对准确，但不适合对大量值进行快速预估。
2、当大于或等于eq_range_index_dive_limit参数阀值时，采用index statistics方式预估影响行数，该方式优点是计算预估值的方式简单，可以快速获得预估数据，但相对偏差较大。

# 3. Insert 语句调优
## 3.1. [`delayed_insert_limit`]

|               Property               |           Value            |
| ------------------------------------ | -------------------------- |
| **Command-Line Format**              | `--delayed-insert-limit=#` |
| **Deprecated**                       | Yes                        |
| **System Variable**                  | `[delayed_insert_limit]`   |
| **Scope**                            | Global                     |
| **Dynamic**                          | Yes                        |
| **[SET_VAR] Hint Applies**           | No                         |
| **Type** (64-bit platforms)          | integer                    |
| **Type** (32-bit platforms)          | integer                    |
| **Default Value** (64-bit platforms) | `100`                      |
| **Default Value** (32-bit platforms) | `100`                      |
| **Minimum Value** (64-bit platforms) | `1`                        |
| **Minimum Value** (32-bit platforms) | `1`                        |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`     |
| **Maximum Value** (32-bit platforms) | `4294967295`               |

## 3.2. [`delay_key_write`]

|          Property          |           Value            |
| -------------------------- | -------------------------- |
| **Command-Line Format**    | `--delay-key-write[=name]` |
| **System Variable**        | `[delay_key_write]`        |
| **Scope**                  | Global                     |
| **Dynamic**                | Yes                        |
| **[SET_VAR] Hint Applies** | No                         |
| **Type**                   | enumeration                |
| **Default Value**          | `ON`                       |
| **Valid Values**           | `ON``OFF``ALL`             |

仅仅用户myISAM表.
如果启用此参数,需要加上`--myisam-recover-options=BACKUP,FORCE`

| Option |                                                                                Description                                                                                 |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `OFF`  | `DELAY_KEY_WRITE` is ignored.                                                                                                                                              |
| `ON`   | MySQL honors any `DELAY_KEY_WRITE` option specified in [`CREATE TABLE`](sql-syntax.html#create-table "13.1.18 CREATE TABLE Syntax") statements. This is the default value. |
| `ALL`  | All new opened tables are treated as if they were created with the `DELAY_KEY_WRITE` option enabled.                                                                       |

## 3.3. [`bulk_insert_buffer_size`]

|               Property               |             Value             |
| ------------------------------------ | ----------------------------- |
| **Command-Line Format**              | `--bulk-insert-buffer-size=#` |
| **System Variable**                  | `[bulk_insert_buffer_size]`   |
| **Scope**                            | Global, Session               |
| **Dynamic**                          | Yes                           |
| **[SET_VAR] Hint Applies**           | Yes                           |
| **Type** (64-bit platforms)          | integer                       |
| **Type** (32-bit platforms)          | integer                       |
| **Default Value** (64-bit platforms) | `8388608`                     |
| **Default Value** (32-bit platforms) | `8388608`                     |
| **Minimum Value** (64-bit platforms) | `0`                           |
| **Minimum Value** (32-bit platforms) | `0`                           |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`        |
| **Maximum Value** (32-bit platforms) | `4294967295`                  |
myisam表专用,用于树状结构插入更快.INSERT ... SELECT, INSERT ... VALUES (...), (...), ..., and LOAD DATA INFILE 语句插入飞空表,

## 3.4. [`concurrent_insert`]

|                                       Property                                        |             Value              |
| ------------------------------------------------------------------------------------- | ------------------------------ |
| **Command-Line Format**                                                               | `--concurrent-insert[=#]`      |
| **System Variable**                                                                   | `[concurrent_insert]`          |
| **Scope**                                                                             | Global                         |
| **Dynamic**                                                                           | Yes                            |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                                                                              | enumeration                    |
| **Default Value**                                                                     | `AUTO`                         |
| **Valid Values**                                                                      | `NEVER``AUTO``ALWAYS``0``1``2` |

当concurrent_insert=0时，不允许并发插入功能。
当concurrent_insert=1时，允许对没有洞洞的表使用并发插入，新数据位于数据文件结尾（缺省）。
当concurrent_insert=2时，不管表有没有洞洞，都允许在数据文件结尾并发插入。
::: alert-warning
此选项为 MyISAM 专用.
::: 

# 4. 语句超时调优
## 4.1. [`have_statement_timeout`]

|          Property          |           Value           |
| -------------------------- | ------------------------- |
| **System Variable**        | `[have_statement_timeout]` |
| **Scope**                  | Global                    |
| **Dynamic**                | No                       |
| **[SET_VAR] Hint Applies** | No                        |
| **Type**                   | boolean                   |

语句执行超时是否启用.
# 5. select  语句调优
## 5.1. [`histogram_generation_max_mem_size`]

|               Property               |                  Value                  |
| ------------------------------------ | --------------------------------------- |
| **Command-Line Format**              | `--histogram-generation-max-mem-size=#` |
| **Introduced**                       | 8.0.2                                   |
| **System Variable**                  | `[histogram_generation_max_mem_size]`   |
| **Scope**                            | Global, Session                         |
| **Dynamic**                          | Yes                                     |
| **[SET_VAR] Hint Applies**           | No                                      |
| **Type** (64-bit platforms)          | integer                                 |
| **Type** (32-bit platforms)          | integer                                 |
| **Default Value** (64-bit platforms) | `20000000`                              |
| **Default Value** (32-bit platforms) | `20000000`                              |
| **Minimum Value** (64-bit platforms) | `1000000`                               |
| **Minimum Value** (32-bit platforms) | `1000000`                               |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`                  |
| **Maximum Value** (32-bit platforms) | `4294967295`                            |
histogram_generation_max_mem_size。当用户建立统计直方图，这个值是用来控制大约多少内存能允许被使用。那么，为什么要控制这个呢？

当你在建立直方图的时候，MySQL server会将所有数据读到内存中，然后在内存中进行操作，包括排序。如果对一个很大的表建立直方图，可能会有风险将几百M的数据都读到内存中，但这是不明智的。为了规避这个风险，MySQL会根据给定的histogram_generation_max_mem_size的值计算该将多少行数据读到内存中。如果根据当前histogram_generation_max_mem_size的限制，MySQL认为只能读一部分数据，那么MySQL会进行取样。通过“sampling-rate”属性，可以观察到取样比率。详情请见:

本地连接:
[D:/SynologyDrive/vnote_notebooks/工作/MySQL/使用文档/直方图统计.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/使用文档/直方图统计.md)

网页连接:
[http://aming.ddns.net:8900/#!MySQL/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3/测试数据导入.md](http://aming.ddns.net:8900/#!MySQL/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3/测试数据导入.md)

[`key_buffer_size`]

|               Property               |         Value          |
| ------------------------------------ | ---------------------- |
| **Command-Line Format**              | `--key-buffer-size=#`  |
| **System Variable**                  | `[key_buffer_size]`    |
| **Scope**                            | Global                 |
| **Dynamic**                          | Yes                    |
| **[SET_VAR] Hint Applies**           | No                     |
| **Type** (64-bit platforms)          | integer                |
| **Type** (32-bit platforms)          | integer                |
| **Default Value** (64-bit platforms) | `8388608`              |
| **Default Value** (32-bit platforms) | `8388608`              |
| **Minimum Value** (64-bit platforms) | `8`                    |
| **Minimum Value** (32-bit platforms) | `8`                    |
| **Maximum Value** (64-bit platforms) | `OS_PER_PROCESS_LIMIT` |
| **Maximum Value** (32-bit platforms) | `4294967295`           |

MyISAM 表的索引块时缓冲到内存里的,并且所有线程都可以共享,key_buffer_size 使用与设置缓存区大小的参数,

此值在32 为系统上最大能设置为4GB -1 .对于64 位系统能够设置更大.
取决于你的物理内存和操作系统对用户和进程的限制.服务器尽可能多分配内存.

对于读取和多次写入的索引的表,可以增加这个值.在一个主要是MyISAM 的存储表上,可以设置这个值为机器内存的25%.如果设置的过大的话(唱过50%)系统可能开始分页并且变得非常慢.因为MySQL 依赖操作系统的读取文件的缓存.所以要给文件缓存系统保留一定的空间.

可以用show status 语句检查键缓冲区的性能.
例如:
```sql
mysql> show status like '%key_read%';
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| Key_read_requests | 0     |
| Key_reads         | 0     |
+-------------------+-------+
2 rows in set (0.01 sec)
```
Key_read/Key_read_repuests 比率应该小于0.1 ,如果经常更新和删除.呢么比率应该接近于1. 如果同时更新许多行,或使用DELAY_LEY_WRITE ,这个比率应该小的多.

## 5.2. [`key_cache_age_threshold`]

|               Property               |             Value             |
| ------------------------------------ | ----------------------------- |
| **Command-Line Format**              | `--key-cache-age-threshold=#` |
| **System Variable**                  | `[key_cache_age_threshold]`   |
| **Scope**                            | Global                        |
| **Dynamic**                          | Yes                           |
| **[SET_VAR]) Hint Applies**          | No                            |
| **Type** (64-bit platforms)          | integer                       |
| **Type** (32-bit platforms)          | integer                       |
| **Default Value** (64-bit platforms) | `300`                         |
| **Default Value** (32-bit platforms) | `300`                         |
| **Minimum Value** (64-bit platforms) | `100`                         |
| **Minimum Value** (32-bit platforms) | `100`                         |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`        |
| **Maximum Value** (32-bit platforms) | `4294967295`                  |

控制键缓存区的内容更新换代.100 更新cache 慢. 设置300 更新cache 快.例如:1 分钟前存 了缓存数据.这个数据现在时hot 子,但是当 100 秒过后没有再使用这个缓存.则这个缓存将会降级,变为worm子.
## 5.3. [`key_cache_block_size`]

|          Property          |           Value            |
| -------------------------- | -------------------------- |
| **Command-Line Format**    | `--key-cache-block-size=#` |
| **System Variable**        | `[key_cache_block_size]`   |
| **Scope**                  | Global                     |
| **Dynamic**                | Yes                        |
| **[SET_VAR] Hint Applies** | No                         |
| **Type**                   | integer                    |
| **Default Value**          | `1024`                     |
| **Minimum Value**          | `512`                      |
| **Maximum Value**          | `16384`                    |

键缓存中的块大小.此值设置的越大,对于更大的块会导致读取操作的数量更少(因为每次读取会获得更多的键)，但是反过来，未检查的键的读取量会增加(如果一个块中的所有键都与查询相关)。

## 5.4. [`key_cache_division_limit`]

|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **Command-Line Format**    | `--key-cache-division-limit=#` |
| **System Variable**        | `[key_cache_division_limit]`   |
| **Scope**                  | Global                         |
| **Dynamic**                | Yes                            |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | integer                        |
| **Default Value**          | `100`                          |
| **Minimum Value**          | `1`                            |
| **Maximum Value**          | `100`                          |
该值是要用于warm子列表的缓冲区列表的百分比。允许值范围从1到100。默认值是100。

## 5.5. [`low_priority_updates`]

|          Property          |          Value           |
| -------------------------- | ------------------------ |
| **Command-Line Format**    | `--low-priority-updates` |
| **System Variable**        | `[low_priority_updates]` |
| **Scope**                  | Global, Session          |
| **Dynamic**                | Yes                      |
| **[SET_VAR] Hint Applies** | No                       |
| **Type**                   | boolean                  |
| **Default Value**          | `FALSE`                  |

如果设置为1  在select  语句时 ,会在整张表上加上 `LOCK TABLE WRITE` 锁, 以提高读效率.(such as `MyISAM`, `MEMORY`, and `MERGE`) 比如这些存储引擎的表.

# 6. 统计信息调优
## 6.1. [`information_schema_stats_expiry`]

|          Property          |                   Value                   |
| -------------------------- | ----------------------------------------- |
| **Command-Line Format**    | `--information-schema-stats-expiry=value` |
| **Introduced**             | 8.0.3                                     |
| **System Variable**        | `[information_schema_stats_expiry]`       |
| **Scope**                  | Global, Session                           |
| **Dynamic**                | Yes                                       |
| **[SET_VAR] Hint Applies** | No                                        |
| **Type**                   | integer                                   |
| **Default Value**          | `86400`                                   |
| **Minimum Value**          | `0`                                       |
| **Maximum Value**          | `31536000`                                |

此值是用来控制统计信息的生成更新频率参数,用于控制统计信息的相关表统计信息的生成频率.
在INFORMATION_SCHEMA 数据库下以下表的字段是包含统计信息的数据.

```
STATISTICS.CARDINALITY
TABLES.AUTO_INCREMENT
TABLES.AVG_ROW_LENGTH
TABLES.CHECKSUM
TABLES.CHECK_TIME
TABLES.CREATE_TIME
TABLES.DATA_FREE
TABLES.DATA_LENGTH
TABLES.INDEX_LENGTH
TABLES.MAX_DATA_LENGTH
TABLES.TABLE_ROWS
TABLES.UPDATE_TIME
```
对于 STATISTICS 表的详细说明请见:
本地:
[D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据字典表/information_schema.statistics.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据字典表/information_schema.statistics.md)
网页版:
[http://aming.ddns.net:8900/#!MySQL/数据字典表/information_schema.statistics.md](http://aming.ddns.net:8900/#!MySQL/数据字典表/information_schema.statistics.md)
对于TABLES 表的字段详细说明请见:
本地:
[D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据字典表/information_schema.tables.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据字典表/information_schema.tables.md)
网页:
[http://aming.ddns.net:8900/#!MySQL/数据字典表/information_schema.tables.md](http://aming.ddns.net:8900/#!MySQL/数据字典表/information_schema.tables.md)

以上这些表的字段将会随着业务表的内容的变化,进行统计信息收集.

默认情况下,MySQL 是从mysql.index_stats 这张表里提取统计信息 .这种方式是降低资源消耗.直到缓存过期,MySQL 才会重新生成统计信息.

统计信息默认更新周期为24 H ,可以设置长达1年.

如果想手动更新统计信息,请使用 ANALYZE TABLE 语句.

如果不想使用统计信息,直接使用存储引擎检索的话.请将 此值设置为0.

当在以下情况,查询统计信息列不会更新统计信息.

- 当统计信息未过期.

- 当information_schema_stats_expiry 设置为0.

- 当服务器启动为read_only.

- 查询性能相关的数据时.

information_schema_stats_expiry 是一个会话变量.存储检索的会话缓存的统计信息可以和其他用户公用.
# 7. jion 语句调优
## 7.1. [`join_buffer_size`]

|                  Property                   |         Value          |
| ------------------------------------------- | ---------------------- |
| **Command-Line Format**                     | `--join-buffer-size=#` |
| **System Variable**                         | `[join_buffer_size]`   |
| **Scope**                                   | Global, Session        |
| **Dynamic**                                 | Yes                    |
| **[SET_VAR] Hint Applies**                  | Yes                    |
| **Type** (Other, 64-bit platforms)          | integer                |
| **Type** (Other, 32-bit platforms)          | integer                |
| **Type** (Windows)                          | integer                |
| **Default Value** (Other, 64-bit platforms) | `262144`               |
| **Default Value** (Other, 32-bit platforms) | `262144`               |
| **Default Value** (Windows)                 | `262144`               |
| **Minimum Value** (Other, 64-bit platforms) | `128`                  |
| **Minimum Value** (Other, 32-bit platforms) | `128`                  |
| **Minimum Value** (Windows)                 | `128`                  |
| **Maximum Value** (Other, 64-bit platforms) | `18446744073709547520` |
| **Maximum Value** (Other, 32-bit platforms) | `4294967295`           |
| **Maximum Value** (Windows)                 | `4294967295`           |

此参数用户普通索引,范围索引,全表扫描的join 语句的连接,而设置的缓冲区大小.通常join 语句最快的方法时添加索引.如果无法添加索引,则可以增加join_buffer_size 的值.可以加快Join 语句的连接速度.

除非是批处理sql (非BKA).否则设置缓冲区大于join 语句所需要的大小.则会拖慢join 语句执行速度.所有连接到MySQL 服务器上的连接都会分配最小值的缓冲区.所以设置这个参数要小心,如果你设置此值为最大值.则会导致性能显著下降.

使用BKA 时 ,join_buffer_size 的值定义了每次批处理的批量大小.缓冲区越大,join 操作右表的顺序访问就越多.可以显著提高性能.

默认值为256kb ,join_buffer_size 最大孕育设置4G -1 (window 除外)

# 8. 慢速日志
## 8.1. long_query_time

|          Property          |         Value         |
| -------------------------- | --------------------- |
| **Command-Line Format**    | `--long-query-time=#` |
| **System Variable**        | `[long_query_time]`   |
| **Scope**                  | Global, Session       |
| **Dynamic**                | Yes                   |
| **[SET_VAR] Hint Applies** | No                    |
| **Type**                   | numeric               |
| **Default Value**          | `10`                  |
| **Minimum Value**          | `0`                   |

sql 语句如果超过了设定的时间,则会把该sql 写入到慢速查询语句中.这个时是时时测量. 设定值为0 到10 秒,可以指定微秒级别,
    case1: 记录到文件为微秒.
    case2: 记录到表只到秒级,微秒将被忽略.

## 8.2. [`log_slow_admin_statements`]

|          Property           |             Value             |
| --------------------------- | ----------------------------- |
| **System Variable**         | `[log_slow_admin_statements]` |
| **Scope**                   | Global                        |
| **Dynamic**                 | Yes                           |
| **[SET_VAR]) Hint Applies** | No                            |
| **Type**                    | boolean                       |
| **Default Value**           | `OFF`                         |


在慢速查询语句中包含以下语句:ALTER TABLE, ANALYZE TABLE, CHECK TABLE, CREATE INDEX, DROP INDEX, OPTIMIZE TABLE, and REPAIR TABLE.


## 8.3. [`log_queries_not_using_indexes`]

|          Property           |               Value               |
| --------------------------- | --------------------------------- |
| **Command-Line Format**     | `--log-queries-not-using-indexes` |
| **System Variable**         | `[log_queries_not_using_indexes]` |
| **Scope**                   | Global                            |
| **Dynamic**                 | Yes                               |
| **[SET_VAR]) Hint Applies** | No                                |
| **Type**                    | boolean                           |
| **Default Value**           | `OFF`                             |


sql 未走索引,如果超过执行的条件那么也会记录到慢速sql 日志中.

## 8.4. [`slow_query_log`]
    
|          Property          |       Value        |
| -------------------------- | ------------------ |
| **Command-Line Format**    | `--slow-query-log` |
| **System Variable**        | `[slow_query_log]` |
| **Scope**                  | Global             |
| **Dynamic**                | Yes                |
| **[SET_VAR] Hint Applies** | No                 |
| **Type**                   | boolean            |
| **Default Value**          | `OFF`              |



此参数为慢速查询日志是否开启, 0 代表 关闭, 1 代表开启, 日志输出目标有log_output 指定,如果输出为NONE ,启用日志也不会写入日志条目.

## 8.5. [`slow_query_log_file`]


|          Property          |               Value               |
| -------------------------- | --------------------------------- |
| **Command-Line Format**    | `--slow-query-log-file=file_name` |
| **System Variable**        | `[slow_query_log_file]`           |
| **Scope**                  | Global                            |
| **Dynamic**                | Yes                               |
| **[SET_VAR] Hint Applies** | No                                |
| **Type**                   | file name                         |
| **Default Value**          | `host_name-slow.log`              |

The name of the slow query log file. The default value is ``*`host_name`*-slow.log``, but the initial value can be changed with the `--slow_query_log_file` option.


慢速查询日志的名字 ,默认值为: ``*`host_name`*-slow.log`` .
## 8.6. [`log_throttle_queries_not_using_indexes`]


|          Property           |                    Value                    |
| --------------------------- | ------------------------------------------- |
| **System Variable**         | `[log_throttle_queries_not_using_indexes])` |
| **Scope**                   | Global                                      |
| **Dynamic**                 | Yes                                         |
| **[SET_VAR]) Hint Applies** | No                                          |
| **Type**                    | integer                                     |
| **Default Value**           | `0`                                         |

如果不打开这个系统变量则不会限制写入慢速查询日志的查询数量,如果为正值,则限制每分钟的写入慢速日志的查询数量.
说明: 只是未走索引的sql 数量.


## 8.7. [`log_slow_slave_statements`]

|          Property          |             Value              |
| -------------------------- | ------------------------------ |
| **System Variable**        | `[log_slow_slave_statements])` |
| **Scope**                  | Global                         |
| **Dynamic**                | Yes                            |
| **[SET_VAR] Hint Applies** | No                             |
| **Type**                   | boolean                        |
| **Default Value**          | `OFF`                          |

使用MySQL 复制的时候 会记录slave 的执行超过指定秒数的sql.



慢速查询日志中的密码将会重写,不会以纯文本形式出现.




慢速查询日志可用于查找执行时间较长的查询,所以可以优化. 但是检查长和慢的查询日志可能很难,因为要执行很长时间. 所以可以使用mysqldumpslow 总结日志中出现的查询1.
详情请见:
本地
[查询慢的sql 的记录日志设置.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/数据库调优/查询慢的sql%20的记录日志设置.md)

[mysqldownslow.md](file:///D:/SynologyDrive/vnote_notebooks/工作/MySQL/程序说明/mysqldownslow.md)
网页:
[http://aming.ddns.net:8900/#!MySQL/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%B0%83%E4%BC%98/查询慢的sql 的记录日志设置.md](http://aming.ddns.net:8900/#!MySQL/数据库调优/查询慢的sql%20的记录日志设置.md)

[http://aming.ddns.net:8900/#!MySQL/%E7%A8%8B%E5%BA%8F%E8%AF%B4%E6%98%8E/mysqldownslow.md](http://aming.ddns.net:8900/#!MySQL/程序说明/mysqldownslow.md)