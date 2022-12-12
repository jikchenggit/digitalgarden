---
{"author":"aming","email":"jikcheng@163.com","title":"Linux SSH Trust","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux SSH Trust","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/sshd","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/sshd/linux-ssh-trust/","dgPassFrontmatter":true}
---


### 配置ssh 互信
#### 192.168.40.112配置 SSH 信任
1. 远端认证
（1）在SSH客户端生成用户的公钥和私钥对文件
```bash
# su - kingbase
$ ssh-keygen -t dsa
```
（2）将SSH客户的公钥添加到SSH服务器中用户的认证文件中
```bash
$ ls -1 ~/.ssh
$  ssh-copy-id -i ~/.ssh/id_dsa.pub 192.168.40.113
```
（3）不需密码或仅需私钥密码就表示密钥认证成功
```bash
$  ssh -l kingbase 192.168.40.113
```
2. 本地认证
（1）将SSH客户的公钥添加到SSH服务器中用户的认证文件中
```bash
# su - kingbase
$  ssh-copy-id -i ~/.ssh/id_dsa.pub 192.168.40.112
```
（2）不需密码或仅需私钥密码就表示密钥认证成功
```bash
# ssh -l kingbase 192.168.40.112
```
#### 192.168.40.113配置 SSH 信任

1、连对端
（1）在SSH客户端生成用户的公钥和私钥对文件
```bash
# su - kingbase
$ ssh-keygen -t dsa
```
（2）将SSH客户的公钥添加到SSH服务器中用户的认证文件中
```bash
$ ls -1 ~/.ssh/
$  ssh-copy-id -i ~/.ssh/id_dsa.pub 192.168.40.112
```
（3）不需密码或仅需私钥密码就表示密钥认证成功
```bash
$  ssh -l kingbase 192.168.40.112
```
2、连本端
（1）将SSH客户的公钥添加到SSH服务器中用户的认证文件中
```bash
# su - kingbase
$ ssh-copy-id -i ~/.ssh/id_dsa.pub 192.168.40.113
```
（2）不需密码或仅需私钥密码就表示密钥认证成功
```bash
$ ssh -l kingbase 192.168.40.113
```