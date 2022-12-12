---
{"author":"aming","email":"jikcheng@163.com","title":"Informix CMD","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Informix CMD","File Folder with relative path":"database/Informix/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/informix/doc/informix-cmd/","dgPassFrontmatter":true}
---


## 查看日志信息
```console
# onstat -m

--------------------------------------------------------------
IBM Informix Dynamic Server Version 11.50.FC8     -- On-Line -- Up 9 days 13:30:30 -- 101950928 Kbytes


Message Log File: /informix/online_gsbys.log
14:22:05  Logical Log 1840403 - Backup Completed
14:25:34  Checkpoint Completed:  duration was 0 seconds.
14:25:34  Fri Mar 29 - loguniq 1840404, logpos 0x75f7018, timestamp: 0xbf3c0cc3 Interval: 833386

14:25:34  Maximum server connections 11340 
14:25:34  Checkpoint Statistics - Avg. Txn Block Time 0.005, # Txns blocked 20, Plog used 67770, Llog used 80743

14:27:38  Logical Log 1840404 Complete, timestamp: 0xbf445257.
14:27:38  Logical Log 1840404 - Backup Started
14:27:47  Logical Log 1840404 - Backup Completed
14:30:05  partition 'cashbill:informix.session_status': no more extents
14:32:29  Logical Log 1840405 Complete, timestamp: 0xbf5700aa.
14:32:29  Logical Log 1840405 - Backup Started
14:32:39  Logical Log 1840405 - Backup Completed
14:35:36  Checkpoint Completed:  duration was 0 seconds.
14:35:36  Fri Mar 29 - loguniq 1840406, logpos 0xa13a2e4, timestamp: 0xbf68366a Interval: 833387

14:35:36  Maximum server connections 11340 
14:35:36  Checkpoint Statistics - Avg. Txn Block Time 0.004, # Txns blocked 22, Plog used 80414, Llog used 113602
```
### 1.1. 查看所有数据库的连接信息

```console
onstat -g sql
```
### 1.2. 查看服务名

```console
dbaccess
```

1. 进入交互式界面,选择database

![](_v_images/20190329145256577_28150.png =850x)
  

2. 选择查看数据库信息

![](_v_images/20190329145403179_23467.png =909x)
  

3.看到当前所有数据库和服务名

![](_v_images/20190329145446629_14850.png =765x)
  

### 1.3. 查询表数据
1. 进入交互式界面,确认查询

![](_v_images/20190329152121995_5852.png =954x)
  

2.键入新的sql 语句

![](_v_images/20190329152221609_20324.png =804x)


  


![](_v_images/20190329152255005_21624.png =876x)
  

3.ESC 完成编辑,RUN 运行

![](_v_images/20190329152326005_9571.png =911x)
  

4.运行结果


![](_v_images/20190329152359240_3482.png =822x)
  


### 1.4. informix 执行数据导出
```sql
unload to /opt/informix/abc.csv delimiter "," select * from test;
```
### 1.5. 数据库字符集
```sql
select * from sysmaster:sysdbslocale;
```
### 1.6. 服务名查询
```console
onstat -c | grep DBSERVER
```
### 1.7. 导出表结构

```console
dbschema -d cust -t interface_mtrlsrv_log interface.sql
```
### 1.8. 外部文件执行
```sql
dbaccess cust test.sql
```
### 1.9. 批量模式显示语句
```console
dbaccess -e metadb dy_table.sql     > dy_log${DATE}y.txt 2>&1
```
### 1.10. 显示所有数据库下所有表
```console
dbschema -d cust  -t all | grep TABLE | awk '{print $3}' | cut -d \. -f 2 > table.txt
```
### 1.11. 查询数据库下所有表





```sql
select dbsname,tabname from sysmaster:systabnames where dbsname='test';
```
### 1.12. 查找主键
```sql
select constrname from sysconstraints where constrtype='P' and tabid= ( select tabid from systables where tabname = 'test' );
```
