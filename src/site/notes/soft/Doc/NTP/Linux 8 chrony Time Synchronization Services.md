---
{"author":"aming","email":"jikcheng@163.com","title":"Linux 8 chrony Time Synchronization Services","creation_date":"2022-10-28 16:59","Last modified date":"2022-11-25 16:01","tags":"Linux 8 chrony Time Synchronization Services","File Folder with relative path":"soft/Doc/NTP","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/ntp/linux-8-chrony-time-synchronization-services/","dgPassFrontmatter":true}
---


###  NTP 服务端
1. 修改`/etc/chrony.conf`
```bash
pool 192.168.10.206 iburst
allow 192.168.10.0/24
```
2. 重启服务
```bash
systemctl  start chronyd
systemctl  status chronyd
systemctl enable chronyd
```
### NTP 客户端
1. 修改`/etc/chrony.conf`

```bash
pool 192.168.10.206 iburst
```
2. 重启服务
```bash
systemctl  start chronyd
systemctl  status chronyd
systemctl enable chronyd
```
### 手动同步一次时间
```bash
chronyc -a makestep
```
### 验证时间同步.
```bash
chronyc sources
chronyc sourcestats
timedatectl
```