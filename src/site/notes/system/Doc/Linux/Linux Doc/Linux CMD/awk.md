---
{"author":"aming","email":"jikcheng@163.com","title":"awk","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"awk","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/awk/","dgPassFrontmatter":true}
---


## awk

```bash
sh insert.sh 2>&1 | sed '/mysql5/d' |  awk '{print "\047"$1"\047","\047"$2"\047","\047"$3"\047","\047"$4"\047","\047"$5"\047","\047"$6"\047","\047"$7"\047","\047"$8"\047","\047"$9"\047
"}'
```


```bash
```bash
sh insert.sh 2>&1 | sed '/mysql5/d' |  awk '{print "insert into data_masking_columns(rule_type,active,table_schema,table_name,column_name,column_comment,create_time,sys_time,instance_i
d) values (\047"$1"\047,","\047"$2"\047,","\047"$3"\047,","\047"$4"\047,","\047"$5"\047,","\047"$6"\047,",$7,",",$8,"\047"$9"\047",$10}'
```