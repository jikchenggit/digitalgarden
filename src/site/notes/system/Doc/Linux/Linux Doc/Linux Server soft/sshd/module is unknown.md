---
{"author":"aming","email":"jikcheng@163.com","title":"module is unknown","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"module is unknown","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/sshd","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/sshd/module-is-unknown/","dgPassFrontmatter":true}
---


- 编辑以下文件
```bash
vi /etc/pam.d/login 
```
- 禁用limits.so 安全模块
```bash
#session required /lib/security/pam_limits.so
#session required pam_limits.so
```
