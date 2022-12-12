---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL oracle sqlplus format log","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"SHELL oracle sqlplus format log","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-oracle-sqlplus-format-log/","dgPassFrontmatter":true}
---



## 1.1. 例子
```console
#!/bin/sh
sqlplus dsg/Zfmjti1_AT << EOF
set pagesize 0
set linesize 400
col table_name for a40
set feedback off;　
set heading off;  
set termout off; 
set trimspool on; 
set echo off;
spool 600.sql
select table_name from META82_DSG_SYNC where id <=600;
spool OFF;
exit;
EOF

sed -i '/SQL>/d;s/^/select first 1 * from  &/g;/^*$/d;s/ *$//;s/$/&;/g'  ./600.sql　　　　 #格式化输出
# 拷贝到源端
scp 600.sql zte_jzf@135.129.24.82:/home/zte_jzf/dsg/ifm_hp_82_s1/
```