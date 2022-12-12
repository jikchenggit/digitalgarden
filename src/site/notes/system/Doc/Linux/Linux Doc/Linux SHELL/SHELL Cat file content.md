---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Cat file content","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL Cat file content","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-cat-file-content/","dgPassFrontmatter":true}
---


* 从第3000行开始，显示1000行。即显示3000~3999行

```sh
cat filename | tail -n +3000 | head -n 1000
```


* 显示1000行到3000行

```sh
cat filename| head -n 3000 | tail -n +1000
```


*注意两种方法的顺序


> 分解：

    tail -n 1000：显示最后1000行

    tail -n +1000：从1000行开始显示，显示1000行以后的

    head -n 1000：显示前面1000行


