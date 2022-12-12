---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Install Chinese Package","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Install Chinese Package","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/linux-install-chinese-package/","dgPassFrontmatter":true}
---


- 修改某个用户的环境变量
```
LANG="zh_CN.UTF-8"
```
- 查看是否正确
```
locale
```
- 查看所有语言包
```sh
locale -a
```
- 安装语言包
```sh
yum install kde-l10n-Chinese
yum reinstall glibc-common
```