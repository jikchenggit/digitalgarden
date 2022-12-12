---
{"author":"aming","email":"jikcheng@163.com","title":"rm","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"rm","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/rm/","dgPassFrontmatter":true}
---



删除除开某个文件中,其他的所有文件.
```bash

shopt -s extglob      （打开extglob模式）
rm -fr !(file1)            #删除keep文件之外的所有文件
rm -rf !(keep1 | keep2) #删除keep1和keep2文件之外的所有文件
```
方法2:
```bash
 ls | grep -v keep | xargs rm 
```
方法3:
```bash
find ./test/ | grep -v keep | xargs rm
```

方法4:
```bash
rm `ls | grep -v"aa"`   #包含aa 字段的 其他都删除.
rm `ls | grep -v"^aa$" `  # 完全匹配.
```
方法5: 
```bash
find ./ -name '[^k][^e][^e][^p]*'  | xargs rm -rf
```