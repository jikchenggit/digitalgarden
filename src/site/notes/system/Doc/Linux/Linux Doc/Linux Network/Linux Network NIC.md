---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Network NIC","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"Linux Network NIC","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Network","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-network/linux-network-nic/","dgPassFrontmatter":true}
---


## LINUX 7.1 添加网卡
* 拷贝网卡文件
```sh
cp ifcfg-eno16777736 ifcfg-eno50332208
```
* 修改配置
```sh
root@ds network-scripts]# vi ifcfg-eno50332208 
TYPE="Ethernet"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
NAME="eno50332208"
#UUID="85cd2aa7-4da7-43a4-af82-64ce33080775"
DEVICE="eno50332208"
ONBOOT="yes"
IPADDR="192.168.2.120"
PREFIX="24"
GATEWAY="192.168.2.2"
DNS1="192.168.2.2"
IPV6_PEERDNS="yes"
IPV6_PEERROUTES="yes"
IPV6_PRIVACY="no"
```

* 启用网卡
```sh
ifup eno50332208
```

# Linux 9.0
## 网卡命令
```bash
/etc/NetworkManager/system-connections/
```

