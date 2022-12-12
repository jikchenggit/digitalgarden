---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL batch remove the file","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"SHELL batch remove the file","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-batch-remove-the-file/","dgPassFrontmatter":true}
---



```sh
find . -name "._*" | xargs rm -rf
```