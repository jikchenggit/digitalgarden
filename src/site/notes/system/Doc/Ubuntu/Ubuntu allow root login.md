---
{"author":"aming","email":"jikcheng@163.com","title":"Ubuntu allow root login","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Ubuntu allow root login","File Folder with relative path":"system/Doc/Ubuntu","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/ubuntu/ubuntu-allow-root-login/","dgPassFrontmatter":true}
---


 - 开启su 登录
```sh
sudo passwd root
```
- ssh 登录开启
- 安装ssh 服务
```sh
apt-get install openssh-server
```

```sh
vi /etc/ssh/sshd_config
```

```sh
# 2. Authentication:
LoginGraceTime 120
#PermitRootLogin without-password
PermitRootLogin yes
StrictModes yes
```