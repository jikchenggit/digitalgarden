---
{"author":"aming","email":"jikcheng@163.com","title":"Memory Performance Tuning","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 13:54","tags":"Memory Performance Tuning","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/memory-performance-tuning/","dgPassFrontmatter":true}
---




##  数据库调优参数
###  [`end_markers_in_json`]

|          Property          |          Value          |
| -------------------------- | ----------------------- |
| **System Variable**        | `[end_markers_in_json]` |
| **Scope**                  | Global, Session         |
| **Dynamic**                | Yes                     |
| **[SET_VAR] Hint Applies** | Yes                     |
| **Type**                   | boolean                 |
| **Default Value**          | `OFF`                   |

此选项是为了调优输出相关信息所用.
1 .打开调优选项。
```sql
SET optimizer_trace="enabled=on";
```
2.查询优化器。
```sql
SELECT * FROM INFORMATION_SCHEMA.OPTIMIZER_TRACE;
```
3.关闭。
```sql
SET optimizer_trace="enabled=off";
```
4. 查看调优器是否开启。
```sql
mysql> show variables like '%optimizer_trace%';
+------------------------------+----------------------------------------------------------------------------+
| Variable_name                | Value                                                                      |
+------------------------------+----------------------------------------------------------------------------+
| optimizer_trace              | enabled=on,one_line=off                                                    |
| optimizer_trace_features     | greedy_search=on,range_optimizer=on,dynamic_range=on,repeated_subselect=on |
| optimizer_trace_limit        | 1                                                                          |
| optimizer_trace_max_mem_size | 1048576                                                                    |
| optimizer_trace_offset       | -1                                                                         |
+------------------------------+----------------------------------------------------------------------------+
5 rows in set (0.00 sec)
```


> [!note]
>  enabled=on 说明调优器打开的,one_line=off,如果是ON 则会以JSON 方式存储数据.但是比较难于查看.


6. 打开one_line 
```sql
SET GLOBAL optimizer_trace="one_line=on";
```
7.查看优化信息
```sql
select * from information_schema.OPTIMIZER_TRACE\G;
```



调优其他参数:

| 参数名称                          | 说明                             |
| --------------------------------- | -------------------------------- |
| --optimizer-trace=name            | 控制优化的跟踪                   |
| --optimizer-trace-features=name   | 是否开启trace 性能跟踪器         |
| --optimizer-trace-limit=          | 显示优化追踪器的最大数量         |
| --optimizer-trace-max-mem-size=   | 存储优化的痕迹允许的最大尺寸累积 |
| --optimizer-trace-offset=         | trace 信息显示的偏移量           |
| see manual --end-markers-in-json= | 使用JSON 格式输出文本。          | 


###  [`flush`]

|         Property          |   Value   |
| ------------------------- | --------- |
| **Command-Line Format**   | `--flush` |
| **System Variable**       | `[flush]` |
| **Scope**                 | Global    |
| **Dynamic**               | Yes       |
| **[SET_VAR]Hint Applies** | No        |
| **Type**                  | boolean   |
| **Default Value**         | `OFF`     |
如果 此变量打开为`ON`,服务将会把所有每个执行过的sql 立刻写入磁盘.如果是OFF,mysql 调用操作系统接口进行数据写入.

> [!note]
> 如果flush 参数enable,fulsh_time 则在对系统无任何影响.
> 

###  [`flush_time`]

|          Property           |                            Value                             |
| --------------------------- | ------------------------------------------------------------ |
| **Command-Line Format**     | `--flush-time=#`                                             |
| **System Variable**         | `[flush_time](server-administration.html#sysvar_flush_time)` |
| **Scope**                   | Global                                                       |
| **Dynamic**                 | Yes                                                          |
| **[SET_VAR]Hint Applies**   | No                                                           |
| **Type** (Windows)          | integer                                                      |
| **Type**                    | integer                                                      |
| **Default Value** (Windows) | `0`                                                          |
| **Default Value**           | `0`                                                          |
| **Minimum Value** (Windows) | `0`                                                          |
| **Minimum Value**           | `0`                                                          |

如果设置这个参数未非0值,所有表将等待 N 秒写入所有未同步flush 数据到磁盘上,此选项最好在资源少的系统上使用.


> [!note]
> 如果flush 参数enable,fulsh_time 则在对系统无任何影响.
> 


###  [`host_cache_size`]

|         Property          |       Value        |
| ------------------------- | ------------------ |
| **System Variable**       | `[host_cache_size` |
| **Scope**                 | Global             |
| **Dynamic**               | Yes                |
| **[SET_VAR]Hint Applies** | No                 |
| **Type**                  | integer            |
| **Default Value**         | `-1 (autosized)`   |
| **Minimum Value**         | `0`                |
| **Maximum Value**         | `65536`            |
设置host cache 的大小,对于服务器的影响详情请见:
[[database/MySQL/Doc/MySQL administration/DNS 解析 和Host Cache\|DNS 解析 和Host Cache]]

 - 设置为0 则是禁用host cache ,在服务运行期间改变当前的host cache 大小则会导致立即清空当前host cache 内存,并且清空记录host  cache 的记录表

- 默认值为128,此参数和max_connections 配合使用,max_connections 值超过500 时每增加20.
此参数加1,上限为2000.

 - 使用 --skip-host-cache 选项可以禁用host cache ,但是host cache 可以在服务运行时,调整缓存大小,并且启用或者禁用主机缓存.

- 如果使用 --skip-host-cache 选项,不会影响到host_cache_size 的更改.但是这些更改没有作用.即使设置为大于0,也不会启用缓存.

