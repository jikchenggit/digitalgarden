---
{"author":"aming","email":"jikcheng@163.com","title":"route","creation_date":"2022-10-12 15:02","Last modified date":"2022-11-25 16:11","tags":"route","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/route/","dgPassFrontmatter":true}
---




## 查看当前Linux 操作系统的网关
```bash
route -n
ip route show
traceroute www.prudentwoo.com -s 100 【第一行就是自己的网关】
netstat -r
more /etc/network/interfaces 【Debian/Ubuntu Linux】
more /etc/sysconfig/network-scripts/ifcfg-eth0 【Red Hat Linux】
```