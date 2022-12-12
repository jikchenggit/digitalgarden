---
{"author":"aming","email":"jikcheng@163.com","title":"logroate","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:04","tags":"logroate","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/logroate/","dgPassFrontmatter":true}
---


```bash

cat /etc/cron.daily/logrotate 
du -sm /data/mysql/logs/error.log 
du -sm /data/mysql/logs/slow.log 


du -sm /data/mysql/log/error.log 
du -sm /data/mysql/log/slow.log 

        # just if mysqld is really running
        if test -x /usr/bin/mysqladmin && \
           /usr/bin/mysqladmin ping &>/dev/null
        then
           /usr/bin/mysqladmin flush-logs
        fi


SFTP 
lcd "D:\telegraf"
cd /etc/logrotate.d
put mysql_error mysql_slow


show variables like '%log_error%';


vi /etc/logrotate.d/mysql_error
du -sm /var/log/mysqld.log


/data/mysql/data/yhjr-hs-mysql02-prd-slow.log



/data/mysql/logs/slow.log.old
```