---
{"author":"aming","email":"jikcheng@163.com","title":"Test the table is active","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:06","tags":"Test the table is active","File Folder with relative path":"database/Informix/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/informix/doc/test-the-table-is-active/","dgPassFrontmatter":true}
---


##  检测同步表是否动态
```console
#!/usr/bin/ksh
dbaccess metadb dy_table.sql     > dy_log20190401x.txt 2>&1 
sleep 60
dbaccess metadb dy_table.sql     > dy_log20190401y.txt 2>&1
diff dy_log20190401x.txt dy_log20190401y.txt
```