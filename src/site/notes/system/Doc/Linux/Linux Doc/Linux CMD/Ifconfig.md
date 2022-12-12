---
{"author":"aming","email":"jikcheng@163.com","title":"Ifconfig","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:06","tags":"Ifconfig","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/ifconfig/","dgPassFrontmatter":true}
---


## Ifconfig
```shell
yum provides ifconfig
```
* 输出样例：
或者你也可以使用以下命令。
```shell
yum whatprovides ifconfig
```
>  这里，“provides”或者“whatprovides”开关用于找出某个包提供了某些功能或文件.  
> 就像你在上面的输出中所看到的，net-tools包提供了ifconfig命令。因此，让我们安装net-tools包来使用ifconfig命令。 

```bash
yum install net-tools
```