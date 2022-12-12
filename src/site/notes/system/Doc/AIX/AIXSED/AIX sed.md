---
{"author":"aming","email":"jikcheng@163.com","title":"AIX sed","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"AIX sed","File Folder with relative path":"system/Doc/AIX/AIXSED","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/aix/aixsed/aix-sed/","dgPassFrontmatter":true}
---



## 删除匹配以外的字段 
```console
sed '/No SELECT permission/!d
```
## 删除行头字符
```console
sed  's/^  272: No SELECT permission for //g'
```
##  删除行尾字符
```console
 sed  's/\.$ //g'
```
## 替换行头
```console
sed "s/^  272: No SELECT permission for /perl -i -pe  \"s\/\'/g"
```
## 替换行尾
```console
sed  "s/\.$/',\/\/g\" dexporter.ini  /g"
```

## 显示上下文
```bash
# m 为显示关键字以下10行,n为显示关键字以上10行.
m=10
n=10
# 确定关键字的最后一个匹配的行数
a=$(sed  -n  "/Sum/="  log.dexporter | tail -1) 
# 显示指定行数上下10行.
sed -n "$((a-m)),$((a+n))"p log.dexporter
```