---
{"author":"aming","email":"jikcheng@163.com","title":"view database size","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"view database size","File Folder with relative path":"database/Informix/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/informix/doc/view-database-size/","dgPassFrontmatter":true}
---

# 1. 查看数据库大小
## 1.1. 检查块大小
```console
oncheck -pr|grep Page
```

::: alert-info
由块大小决定数据大小
:::
## 1.2. 查看当前所有数据库
```sql
SELECT dbsname[1,15] database_name, SUM(pe_size)*4/1024/1024 size_GB
FROM sysmaster:sysptnext,
OUTER sysmaster:systabnames
WHERE pe_partnum = partnum
GROUP BY 1
ORDER BY 2 DESC;
```
## 1.3. 查看单个数据库空间
```sql
select dbsname,count(*) num_of_extents,sum(pe_size)*4/1024/1024 size_GB
from sysmaster:systabnames,sysmaster:sysptnext
where partnum = pe_partnum and dbsname="cust"
group by 1
order by 2 desc,3 desc;
```
## 1.4. 查看数据库下每张表大小
```sql
select dbsname,tabname,count(*) num_of_extents,sum(pe_size)*4/1024/1024 size_GB
from sysmaster:systabnames,sysmaster:sysptnext
where partnum = pe_partnum and dbsname="cust"
group by 1,2
order by 4 desc;
```
## 1.5. 查询单张表大小
```sql
select dbsname,tabname,sum(size)*4/1024/1024 size_GB
from sysmaster:sysextents
where tabname[1,3] <> 'sys'
and dbsname='cust' and tabname='serv_state_attr'
group by 1,2
order by 3 desc;
```