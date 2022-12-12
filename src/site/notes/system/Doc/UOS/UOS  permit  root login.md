---
{"author":"aming","email":"jikcheng@163.com","title":"UOS  permit  root login","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"UOS  permit  root login","File Folder with relative path":"system/Doc/UOS","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/uos/uos-permit-root-login/","dgPassFrontmatter":true}
---


## 允许root 登录

```bash
vi /etc/ssh/sshd_config 
Authentication:

LoginGraceTime 120
PermitRootLogin yes
StrictModes yes
```
