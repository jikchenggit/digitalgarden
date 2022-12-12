---
{"author":"aming","email":"jikcheng@163.com","title":"Linux timeout login","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux timeout login","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/sshd","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/sshd/linux-timeout-login/","dgPassFrontmatter":true}
---


# 1. 登录超时
## 1.1. 方案一：在客户端设置
方法很简单，只需在客户端电脑上编辑（需要root权限）/etc/ssh/ssh_config，并添加如下一行：

* CRT 设置


```sh
ServerAliveInterval 60
```
此后该系统里的用户连接SSH时，每60秒会发一个KeepAlive请求，避免被踢。

## 1.2. 方案二：在服务器端设置
如果有相应的权限，也可以在服务器端设置，即编辑/etc/ssh/sshd_config，并添加：

```sh
ClientAliveInterval 60
```
重启SSH服务器后该项设置会生效。每一个连接到此服务器上的客户端都会受其影响。应注意启用该功能后，安全性会有一定下降（比如忘记登出时……）
