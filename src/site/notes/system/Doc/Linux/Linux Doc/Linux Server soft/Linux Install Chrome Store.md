---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Install Chrome Store","creation_date":"2022-10-20 09:48","Last modified date":"2022-11-25 16:00","tags":"Linux Install Chrome Store","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/linux-install-chrome-store/","dgPassFrontmatter":true}
---


## 上传谷歌浏览器
```bash
[root@node1 ~]# ls -ltra google-chrome-stable_current_x86_64.rpm 
-rw-r--r-- 1 root root 89356464 8月  12 13:06 google-chrome-stable_current_x86_64.rpm
```
## 配置yum 源
```bash
[root@node1 yum.repos.d]# pwd
/etc/yum.repos.d
[root@node1 yum.repos.d]# ls
CentOS-Base.repo  CentOS-CR.repo  CentOS-Debuginfo.repo  CentOS-fasttrack.repo  CentOS-Media.repo  CentOS-Sources.repo  CentOS-Vault.repo
```

> [!warning]
> 
> 默认配置即可.

## 允许虚拟机连接互联网
### 编辑网卡配置文件
```bash
vi /etc/sysconfig/network-scripts/ifcfg-eno16777736 
```
### 修改或添加DNS 和网关。
```

GATEWAY=192.168.40.2
DNS1=192.168.40.2
```

### 重启网络服务
```bash
systemctl restart network;
```
### 能够ping 通外网
```bash
ping www.baidu.com
```

## 安装谷歌浏览器
```bash
[root@node1 ~]# yum localinstall google-chrome-stable_current_x86_64.rpm -y
```

### 完成安装
![](https://www.aming.work:8084/images/2022/10/20/20221020095613.png)
