---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL Stram Ignore Table","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 13:39","tags":"MySQL Stram Ignore Table","File Folder with relative path":"database/MySQL/Doc/MySQL Stream","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-stream/my-sql-stram-ignore-table/","dgPassFrontmatter":true}
---







## 踢表
```bash
stop slave for channel 'master_yhloan';
CHANGE REPLICATION FILTER REPLICATE_IGNORE_TABLE = (yhloan.task_lock,yhloan.seqctl,yhloan.bh_daily_loan_voucher_info,yhloan.tla_ln_flrcv) FOR channel 'master_yhloan';
start slave for channel 'master_yhloan'; 
```