---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL substitute M","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL substitute M","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-substitute-m/","dgPassFrontmatter":true}
---


# 1. 替换^M
## 1.1. 第一种方法： 
```sh
cat -A filename 就可以看到windows下的断元字符 ^M
```
要去除，最简单用下面的命令：
```sh
dos2unix filename
```
## 1.2. 第二种方法：
```sh
sed -i 's/^M//g' filename
```
> 注意：^M的输入方式是 Ctrl + v ，然后Ctrl + M 
 
## 1.3. 第三种方法：
```sh
#vi filename
:1,$ s/^M//g
```

^M 输入方法： 
```sh
ctrl+V ,ctrl+M
```
## 1.4. 第四种方法：
```sh
#cat filename |tr -d '/r' > newfile
#^M 可用 /r 代替
```