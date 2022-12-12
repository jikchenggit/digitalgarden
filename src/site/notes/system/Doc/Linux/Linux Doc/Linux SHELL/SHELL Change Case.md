---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Change Case","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:49","tags":"SHELL Change Case","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-change-case/","dgPassFrontmatter":true}
---


* 转换成大写
```sh
UPCASEVAR=$(echo $VARIABLE | tr '[a-z]' '[A-Z]') 
```
* 转换成小写
```sh
UPCASEVAR=$(echo $VARIABLE | tr '[A-Z]' '[a-z]') 
```
* 变量设置成永是大写(或小写)
```sh
typeset -u VARIABLE
typeset -l VARIABLE
```
