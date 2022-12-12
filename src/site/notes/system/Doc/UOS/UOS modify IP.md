---
{"author":"aming","email":"jikcheng@163.com","title":"UOS modify IP","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"UOS modify IP","File Folder with relative path":"system/Doc/UOS","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/uos/uos-modify-ip/","dgPassFrontmatter":true}
---


## 修改方式(命令行方式)


```bash
nmcli connection modify ens33 ipv4.method manual ipv4.addresse 1.1.1.4/24 ipv4.gateway 1.1.1.1 ipv4.dns 1.1.1.2
```

## 修改方式(配置文件方式)
```bash
[root@localhost ~]# vi /etc/network/interfaces
 
# interfaces(5) file used by ifup(8) and ifdown(8)
# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d
auto enp125s0f0
iface enp125s0f0 inet static
address 192.168.128.172/24
gateway 192.168.128.254
```