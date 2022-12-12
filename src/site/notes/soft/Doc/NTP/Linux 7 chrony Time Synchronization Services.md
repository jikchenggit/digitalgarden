---
{"author":"aming","email":"jikcheng@163.com","title":"Linux 7 chrony Time Synchronization Services","creation_date":"2022-10-28 17:13","Last modified date":"2022-11-27 19:41","tags":"Linux 7 chrony Time Synchronization Services","File Folder with relative path":"soft/Doc/NTP","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/ntp/linux-7-chrony-time-synchronization-services/","dgPassFrontmatter":true}
---


## 安装NTP 软件
```bash
yum install ntpd -y
```
## 配置ntp 服务器
1、以某个节点为 NTP 服务器，编辑/etc/ntp.conf
```bash
 vi /etc/ntp.conf
```

2、加入如下内容，其中 192.168.10.1 为子网 IP，255.255.255.0 为子网掩码
```bash
server 127.0.1.0 prefer # local clock (LCL)  
restrict 192.168.10.1 mask 255.255.255.0
```

| 选项     | 说明                                                                         |
| -------- | ---------------------------------------------------------------------------- |
| server   | 主机需要从哪台机器同步时间。这里指向自身。如果有NTP 服务器请指向NTP 服务器。 |
| fudge    | 不需要和本地同步时间。                                                       |
| restrict | 限制哪些IP 地址能够连接到本台服务                                                                            |

2、启动ntpd 服务

```bash
systemctl start ntpd
systemctl status ntpd
```
3、开机自启

```bash
systemctl enable ntpd
```

## 配置NTP 客户端
1、配置ntp.conf
```bash
 vi /etc/ntp.conf
```
添加如下内容，其中192.168.10.158为NTP 服务器IP地址。
```
server 192.168.10.158 prefer
```

2、启动NTP 客户端


```bash
systemctl start ntpd
systemctl status ntpd
```
3、开机自启

```bash
systemctl enable ntpd
```

## 验证NTP 服务是否正常
```bash
[root@node1 ~]# ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 192.168.10.158  .INIT.          16 u   37   64    0    0.000    0.000   0.000
[root@node1 ~]# 
```