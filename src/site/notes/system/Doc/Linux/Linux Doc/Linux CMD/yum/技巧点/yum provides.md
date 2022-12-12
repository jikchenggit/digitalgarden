---
{"author":"aming","email":"jikcheng@163.com","title":"yum provides","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"yum provides","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD/yum/技巧点","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/yum//yum-provides/","dgPassFrontmatter":true}
---


## 搜索某个命令在哪个软件包里。
```sh
yum provides ifconfig
```
输出样例：
或者你也可以使用以下命令。

```sh
yum whatprovides ifconfig
```
这里，“provides”或者“whatprovides”开关用于找出某个包提供了某些功能或文件。

就像你在上面的输出中所看到的，net-tools包提供了ifconfig命令。因此，让我们安装net-tools包来使用ifconfig命令。
## 全文搜索关键字

```bash
yum search perl  
yum search all qmake
```