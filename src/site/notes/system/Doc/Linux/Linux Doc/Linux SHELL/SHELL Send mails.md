---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Send mails","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL Send mails","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-send-mails/","dgPassFrontmatter":true}
---




```sh
mail -s "This is hte subject" $MAILOUT_LIST < $MAIL_FILE
cat $MAIL_FILE | mail -s " This is the subject" $MAILOUT_LIST
mailx -s "This the subject" $MAILOUT_LIST < $MAIL_FILE
cat $MAIL_FILE | mailx -s "This is the subject" $MAILIOUT_LIST
```
