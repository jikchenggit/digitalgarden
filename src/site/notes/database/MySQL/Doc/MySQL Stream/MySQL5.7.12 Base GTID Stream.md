---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL5.7.12 Base GTID Stream","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:08","tags":"MySQL5.7.12 Base GTID Stream","File Folder with relative path":"database/MySQL/Doc/MySQL Stream","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-stream/my-sql-5-7-12-base-gtid-stream/","dgPassFrontmatter":true}
---






##  开放防火墙端口
```bash
 firewall-cmd --zone=public --add-port=3306/tcp --permanent 
 firewall-cmd --reload
```

## 查看某个端口
```bash
firewall-cmd --zone=public --query-port=80/tcp
```
## 操作系统配置（可选）
```bash
systemctl  stop firewalld
systemctl  disable firewalld
```

# 主端配置

## 设置server_id
```bash
server_id = 1
```
## 开启log-bin

```bash
log-bin=on
```
> 检查是否开启bin log 日志：
```bash
show variables like '%log_bin%';
```
> 此选项是只读选项，需要添加到`my.cnf` 文件中。 且需要重启数据库。
> 确认以下参数为打开1 状态。
```bash
show variables like '%sync_binlog%';
```

```bash
show variables like '%innodb_flush_log_at_trx%';
```

##  配置log-bin 的保留日志
```bash
expire_logs_days=10
```
## 开启gtid 模式
- 编辑`/etc/my.cnf`
```bash
gtid_mode=ON
enforce-gtid-consistency=ON
```
- 重启MySQL·
```bash
systemctl restart mysqld
```

## 在主端上创建从端连接的用户
```bash
CREATE USER 'repl'@'192.168.10.164' IDENTIFIED BY 'repl';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.10.164';
```

## 生成新的uuid
删除`datadir` 中的auto.conf文件。会自动生成新的uuid。
- 重启MySQL·
```bash
systemctl restart mysqld
```

## 制作备份
```bash
mysqldump -uroot -p1234 --all-databases --master-data > dbdump.db
```

> 注意：开启GTID 功能后以前的备份就不能用了。 

# 从库准备
## 数据库初始化
```bash
mysql -u root -p1234
```
```bash
mysql < dbdump.db
```
```bash
CHANGE MASTER TO
MASTER_HOST = '192.168.10.166',
MASTER_PORT = 3306,
MASTER_USER = 'repl',
MASTER_PASSWORD = 'repl',
MASTER_CONNECT_RETRY=10,
MASTER_AUTO_POSITION = 1
FOR CHANNEL 'channel1';
```

> 要想使用多组复制，请把以下参数设置为TABLE ，否则，只能有一个slave 进程。
```bash
master_info_repository=TABLE 
relay_log_info_repository=TABLE
```

所有信息都存入在表中。

```bash
slave_master_info   
slave_relay_log_info
slave_worker_info   
```

清除所有slave 信息
```bash
 reset slave all;
```
##  设置server_id
```bash
server_id = 2
```
# 查询语句

- 主从复制应用状态。
```bash
select * from performance_schema.replication_applier_status_by_worker;
```

- gtid 事务查询
```bash
select * from mysql.gtid_executed;
```

- gitdb 压缩线程查询
```bash
 SELECT * FROM performance_schema.threads WHERE NAME LIKE '%gtid%'\G
```

变量查询
```bash
show variables like  '%gtid_next%';
```

```bash
show variables like '%server_uuid%';
```

- 查询多个变量

```bash
show variables where variable_name in ('server_uuid','gtid_next','gtid_executed' ,'gtid_purged','gtid_executed_compression_period','master_info_repository','relay_log_info_repository');
```

参数说明：
- server_uuid
    服务器唯一标识码
- gtid_executed_compression_period
    gtid_executed 表压缩参数，代表1000 个GITD 事务生成后就进行压缩。
- gtid_executed
    生成的gtid 集合变量。
- gtid_next
     gtid 生成规则。
- gtid_purged 
     gitd 清除规则。

# 设置过滤规则
##  只应用（不应用）某个DB 数据库（MySQL 8.0）
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test) FOR CHANNEL channel1;
```

## 只应用(不应用)某个DB 数据库（MySQL 5.7）
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test),REPLICATE_IGNORE_DB = (ad_test);
```

## 排除数据库（MySQL8.0）
```bash
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) FOR CHANNEL channel1;
```
## 排除数据库（MySQL5.7）
```bash
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) ;
```
## 只应用某些表

```bash
CHANGE REPLICATION FILTER
    REPLICATE_WILD_DO_TABLE = ('ad_test.old%');
```

> 当`REPLICATE_DO_DB` 指定时，这个参数优先级低于`REPLICATE_DO_DB`.

## 从DB1 语句转写到DB2 
```bash
CHANGE REPLICATION FILTER REPLICATE_REWRITE_DB = ((db1, db2));
```

## 同时指定同一个库
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test),REPLICATE_IGNORE_DB = (ad_test) ;
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) ,REPLICATE_DO_DB = (ad_test);
CHANGE REPLICATION FILTER REPLICATE_WILD_IGNORE_TABLE = ('tpcds.test');
```
```bash
mysql> stop slave for channel 'channel1';
Query OK, 0 rows affected (0.03 sec)

mysql> CHANGE REPLICATION FILTER REPLICATE_WILD_IGNORE_TABLE = ('tpcds.test','tpcds.test2');
Query OK, 0 rows affected (0.00 sec)

mysql> start slave for channel 'channel1';
Query OK, 0 rows affected (0.02 sec)
```

> ad_test 下的库所有表都不会过滤。
## 同时指定一个表

```bash
CHANGE REPLICATION FILTER REPLICATE_DO_TABLE = (ad_test),REPLICATE_IGNORE_TABLE = (ad_test) ;
CHANGE REPLICATION FILTER REPLICATE_IGNORE_TABLE = (ad_test),REPLICATE_DO_TABLE = (ad_test) ;
```
## 清空过滤规则

```bash
CHANGE REPLICATION FILTER
    REPLICATE_DO_DB = (), 
    REPLICATE_IGNORE_DB = (),
    REPLICATE_REWRITE_DB=(),
    REPLICATE_WILD_DO_TABLE=(),
    REPLICATE_WILD_IGNORE_TABLE=(),
    REPLICATE_DO_TABLE=(), 
    REPLICATE_IGNORE_TABLE=();
    
```

## 相同语句最后一个生效

```bash
CHANGE REPLICATION FILTER
    REPLICATE_DO_DB = (db1, db2), REPLICATE_DO_DB = (db3, db4);

CHANGE REPLICATION FILTER
    REPLICATE_DO_DB = (db3, db4);
```
> 以上语句生效的只有DB3，和DB4.
