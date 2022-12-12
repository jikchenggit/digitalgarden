---
{"author":"aming","email":"jikcheng@163.com","title":"Linux  Memory","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:04","tags":"Linux  Memory","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Memory","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-memory/linux-memory/","dgPassFrontmatter":true}
---


```bash
# systemctl | grep service | grep runn | awk  -F " "  '{print$1}' > list

# for  v  in `cat list`
do
    echo " $v `systemctl status  $v | grep Memory `" >> log
done
```