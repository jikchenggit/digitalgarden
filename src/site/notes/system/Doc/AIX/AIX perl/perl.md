---
{"author":"aming","email":"jikcheng@163.com","title":"perl","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"perl","File Folder with relative path":"system/Doc/AIX/AIX perl","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/aix/aix-perl/perl/","dgPassFrontmatter":true}
---


## 常用perl 替换
### 匹配指定数目的字符 
   字符对{}指定所匹配字符的出现次数。如：
   /de{1,3}f/匹配def,deef和deeef；
   /de{3}f/匹配deeef；
   /de{3,}f/匹配不少于3个e在d和f之间；
   /de{0,3}f/匹配不多于3个e在d和f之间。
```bash
perl -i -pe  "s/'partition_item_930',{0,1}//g" dexporter.ini 
```
### 批量替换文件中的字符

```bash
perl -i -pe 's/TRFF_ZH/TRFF/g' *
```
### 批量删除换行符

```
perl -lane '{printf $_} ' 
```