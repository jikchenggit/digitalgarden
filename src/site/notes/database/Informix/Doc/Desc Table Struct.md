---
{"author":"aming","email":"jikcheng@163.com","title":"Desc Table Struct","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Desc Table Struct","File Folder with relative path":"database/Informix/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/informix/doc/desc-table-struct/","dgPassFrontmatter":true}
---


## 1. 查看表结构
```sql

SELECT c.colname[1,20], c.coltype, c.collength
FROM syscolumns c, systables t
WHERE c.tabid = t.tabid
AND t.tabname = 'xxxTable';
```
## 1.1. 扫描所有表字段
```sh
dbaccess -e cashbill  table.sql | grep -vE 'Database|^$|colname|SELECT|WHERE|AND|FROM' | awk   '{print $1}' |sed 's/$/,/g'|perl -lane '{printf $_} '  > ji
sed ':a;N;s/\n/ /g;ta' ji 
```