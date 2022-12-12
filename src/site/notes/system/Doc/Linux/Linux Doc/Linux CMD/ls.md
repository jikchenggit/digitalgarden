---
{"author":"aming","email":"jikcheng@163.com","title":"ls","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"ls","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/ls/","dgPassFrontmatter":true}
---


反向显示相关文件
```bash

shopt -u extglob #关闭
shopt -s extglob #打开

ls -al !(*.*) 
ls -al -d !(*.*)
```
