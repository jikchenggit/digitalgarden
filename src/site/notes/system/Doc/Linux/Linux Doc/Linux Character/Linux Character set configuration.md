---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Character set configuration","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:46","tags":"Linux Character set configuration","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Character","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-character/linux-character-set-configuration/","dgPassFrontmatter":true}
---


## 1.1. 查看字符集
```sh
echo $LANG
en_US.UTF-8
```
 
```sh
env |grep LANG
LANG=en_US.UTF-8
```

```sh
locale -a                  //查看本地字符集
locale -m                 //查看所有支持的字符集
```
## 1.2. 设置字符集

- 直接设置变量的方式修改，命令如下两条命令：

```console
[root@ ~]# LANG=xxx           或者  export  LANG=xxx； 
[root@ ~]# LC_ALL=”xxx”  或者  export LC_ALL=”xxx”；
```
注：xxx为欲修改为的字符集
 
查看标准的字符集的方法，locale –a命令，常用的有zh_CN.GB2312、zh_CN.GB18030或者zh_CN.UTF-8、en_US.UTF-8等,但是上述修改方式只会在当前shell中生效，新建shell此环境变量消失。
 
故平时登录系统执行`LANG= `这个命令就没有乱码的缘故，意思就是取消了字符集的显示.
取消字符集还可以执行` unset LANG`这个命令
