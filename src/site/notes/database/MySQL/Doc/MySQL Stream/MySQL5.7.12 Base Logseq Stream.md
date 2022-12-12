---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL5.7.12 Base Logseq Stream","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:08","tags":"MySQL5.7.12 Base Logseq Stream","File Folder with relative path":"database/MySQL/Doc/MySQL Stream","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-stream/my-sql-5-7-12-base-logseq-stream/","dgPassFrontmatter":true}
---





## 操作系统配置
### 开放防火墙端口
```bash
 firewall-cmd --zone=public --add-port=3306/tcp --permanent 
 firewall-cmd --reload
```
### 查看某个端口
```bash
firewall-cmd --zone=public --query-port=80/tcp
```
### 操作系统配置（可选）
```bash
systemctl  stop firewalld
systemctl  disable firewalld
```
## 配置主端准备
### 设置server_id
```bash
server_id = 1;
```
### 开启log-bin

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

### 在主端上创建从端连接的用户
```bash
CREATE USER 'repl'@'192.168.10.164' IDENTIFIED BY 'repl';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.10.164';
```
### 确定主端的postion
1. 锁住所有表，只能读不能写。
```bash
FLUSH TABLES WITH READ LOCK;
```
2. 显示postion 位置
```bash
SHOW MASTER STATUS;
```
###  配置log-bin 的保留日志
```bash
expire_logs_days=10
```
## 配置备端
### 生成新的uuid
删除`datadir` 中的auto.conf文件。会自动生成新的uuid。
###  设置server_id
```bash
server_id = 2
```
> 注意：不能设置为0 ，否则禁止所有链接。
> 注意2：需要不同的
### 初始化数据导入
1. 如果没有存量数据，则可以不进行初始化。
2. 如果有存量数据，请使用`mysqldump`进行数据初始化。


### 制作从库镜像
```bash
mysqldump -uroot -p1234 --all-databases --master-data > dbdump.db
```
### 生成备份
```bash
mysqldump -uroot -p1234 --all-databases --master-data > dbdump.db
```

## 配置从库

### 数据库初始化
```bash
mysql -u root -p1234
```
```bash
mysql < dbdump.db
```
### 创建主从服务器
```bash
CHANGE MASTER TO
MASTER_HOST='192.168.10.163',
MASTER_USER='repl',
MASTER_PASSWORD='repl',
master_log_file='on.000004',
MASTER_LOG_POS=154;
```
## 启动slave 端
```bash
start slave ；
```
### 查看slave 端是否正常
```bash
show slave status\G
```
## 配置到此完成
## 经验
###  丢数据场景一：
1. 如果binglog 还未应用，就被清除后，会丢数据，且slave 会报对应错误。
2. 可以重新执行binlog 的起始位置。继续同步。
3. 会丢数据。
### 刷新日志进行日志传输
```bash
flush logs;
```

## 设置过滤规则
###  只应用（不应用）某个DB 数据库（MySQL 8.0）
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test) FOR CHANNEL channel1;
```

### 只应用(不应用)某个DB 数据库（MySQL 5.7）
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test),REPLICATE_IGNORE_DB = (ad_test);
```

### 排除数据库（MySQL8.0）
```bash
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) FOR CHANNEL channel1;
```
### 排除数据库（MySQL5.7）
```bash
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) ;
```
### 只应用某些表

```bash
CHANGE REPLICATION FILTER
    REPLICATE_WILD_DO_TABLE = ('ad_test.old%');
```

> 当`REPLICATE_DO_DB` 指定时，这个参数优先级低于`REPLICATE_DO_DB`.

### 从DB1 语句转写到DB2 
```bash
CHANGE REPLICATION FILTER REPLICATE_REWRITE_DB = ((db1, db2));
```

### 同时指定同一个库
```bash
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (ad_test),REPLICATE_IGNORE_DB = (ad_test) ;
CHANGE REPLICATION FILTER REPLICATE_IGNORE_DB = (ad_test) ,REPLICATE_DO_DB = (ad_test);
```

> ad_test 下的库所有表都不会过滤。
### 同时指定一个表

```bash
CHANGE REPLICATION FILTER REPLICATE_DO_TABLE = (ad_test),REPLICATE_IGNORE_TABLE = (ad_test) ;
CHANGE REPLICATION FILTER REPLICATE_IGNORE_TABLE = (ad_test),REPLICATE_DO_TABLE = (ad_test) ;
```
### 清空过滤规则

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

### 相同语句最后一个生效

```bash
CHANGE REPLICATION FILTER
    REPLICATE_DO_DB = (db1, db2), REPLICATE_DO_DB = (db3, db4);

CHANGE REPLICATION FILTER
    REPLICATE_DO_DB = (db3, db4);
```
> 以上语句生效的只有DB3，和DB4.
## 范例
```bash
*************************** 1. row ***************************
             File: mysql-bin.000051
         Position: 37448556
     Binlog_Do_DB: 
 Binlog_Ignore_DB: 
Executed_Gtid_Set: ddb8668b-57e1-11e7-9542-005056a34180:1-46297024
1 row in set (0.00 sec)


CHANGE MASTER TO
MASTER_HOST='192.168.10.163',
MASTER_USER='repl',
MASTER_PASSWORD='repl',
master_log_file='mysql-bin.000008',
MASTER_LOG_POS=320;



CHANGE MASTER TO
MASTER_HOST = '10.1.34.180',
MASTER_PORT = 3306,
MASTER_USER = 'root',
MASTER_PASSWORD = 'JyjflaJLF*fladf123',
master_log_file='mysql-bin.000001',
MASTER_LOG_POS=352751;




CHANGE MASTER TO
MASTER_HOST = '10.1.34.180',
MASTER_PORT = 3306,
MASTER_USER = 'root',
MASTER_PASSWORD = 'JyjflaJLF*fladf123',
MASTER_CONNECT_RETRY=10,
MASTER_AUTO_POSITION = 1;


MASTER_CONNECT_RETRY=10,

change master to MASTER_AUTO_POSITION = 0;


FOR CHANNEL 'channel1';
```