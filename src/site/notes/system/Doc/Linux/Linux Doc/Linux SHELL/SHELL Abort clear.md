---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Abort clear","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL Abort clear","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-abort-clear/","dgPassFrontmatter":true}
---



```sh
trap 'echo "\nEXITING on a TRAPPED SIGNSL";exit'1 2 3 15
```