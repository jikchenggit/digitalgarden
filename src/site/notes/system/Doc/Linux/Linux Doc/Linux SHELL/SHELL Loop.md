---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Loop","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:05","tags":"SHELL Loop","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-loop/","dgPassFrontmatter":true}
---



```sh
[aming@VIM chapter1]$ cat while_output.sh 
while read LINE
do 
        echo "$LINE"
done < <(ls)
```

使用命令在done 尾部 使用命令数据。While 循环会一直读取命令行的输出。
