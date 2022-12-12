---
{"author":"aming","email":"jikcheng@163.com","title":"Informax Database extraction Information","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Informax Database extraction Information","File Folder with relative path":"system/Doc/AIX/AIX SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/aix/aix-shell/informax-database-extraction-information/","dgPassFrontmatter":true}
---


## 1.1. 从日志中提取无权限同步表,然后从配置文件中删除.
```bash
dbaccess metadb 600.sql > log.no_priveli_table 
cat log.no_priveli_table |  sed '/No SELECT permission/!d'  | sed "s/^  272: No SELECT permission for /perl -i -pe  \"s\/\'/g"   |   \
sed  "s/\.$/',\/\/g\" dexporter.ini  /g"  