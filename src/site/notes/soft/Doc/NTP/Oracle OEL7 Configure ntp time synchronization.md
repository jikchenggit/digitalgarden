---
{"author":"aming","email":"jikcheng@163.com","title":"Oracle OEL7 Configure ntp time synchronization","creation_date":"2022-10-28 16:53","Last modified date":"2022-11-27 19:41","tags":"Oracle OEL7 Configure ntp time synchronization","File Folder with relative path":"soft/Doc/NTP","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/ntp/oracle-oel-7-configure-ntp-time-synchronization/","dgPassFrontmatter":true}
---


## Oracle 配置ntp时间同步
 - 安装Oracle 11g RAC时，我们需要配置ntp服务。在使用虚拟机的情况下对于时钟同步方式的配置有很多种方式，可以使用vmware自带的时钟同步功能，也可以直接将本地的一个节点用作时间服务器。本文介绍直接配置ntp方式的时钟服务器。


1、查看两节点的hosts配置  

```bash
 [root@node1 ~]# cat /etc/hosts  
 # Do not remove the following line, or various programs  
 # that require network functionality will fail.  
 #127.0.0.1              localhost.localdomain localhost  
 #::1            localhost6.localdomain6 localhost6  
   
 127.0.0.1       localhost.szdb.com   localhost  
 # Public eth0  
 192.168.7.71   node1.szdb.com        node1  
 192.168.7.72   node2.szdb.com        node2  
   
 #Private eth1  
 10.10.7.71   node1-priv.szdb.com   node1-priv  
 10.10.7.72   node2-priv.szdb.com   node2-priv  
   
 #Virtual  
 192.168.7.81   node1-vip.szdb.com    node1-vip  
 192.168.7.82   node2-vip.szdb.com    node2-vip  
  
```
2、确认各节点的ntp包已经安装  

```bash
 [oracle@node1 ~]$ rpm -qa | grep ntp  
 ntp-4.2.2p1-9.el5_4.1  
 chkfontpath-1.10.1-1.1      #这个是和字体有关，非ntp包  
 [oracle@node1 ~]$ ssh node2 rpm -qa | grep ntp  
 ntp-4.2.2p1-9.el5_4.1  
 chkfontpath-1.10.1-1.1      #这个是和字体有关，非ntp包  
```

3、编辑两节点的ntp.conf文件  
```bash
 [oracle@node1 ~]$ su - root  
 Password:   
 [root@node1 ~]#  vi /etc/ntp.conf  
    
 #New ntp server added by Robinson  
 server  127.127.1.0 prefer  # 添加首选的时钟服务器  
 restrict 192.168.7.0  mask 255.255.255.255 nomodify notrap #只允许192.168.7.*网段的客户机进行时间同步  
 broadcastdelay 0.008  
   
 [root@node2 ~]# vi /etc/ntp.conf  
    
 #New ntp server added by Robinson  
 server 192.168.7.71 prefer  
 broadcastdelay 0.008  
```
   
4、编辑两节点的ntpd参数  
```bash

 [root@node1 ~]# vi /etc/sysconfig/ntpd  
 #The following item added by Robinson  
 #Set to 'yes' to sycn hw clock after successful ntpdate  
 SYNC_HWCLOCK=yes      #此选项用于自动校准系统时钟与硬件时钟  
 OPTIONS="-x -u ntp:ntp -p /var/run/ntpd.pid"  
```
   
 ```
 #注意理解Linux的时钟类型。在Linux系统中分为系统时钟和硬件时钟.  
 #系统时钟指当前Linux kernel中的时钟，而硬件时钟指的是BIOS时钟，由主板电池供电的那个时钟  
 #当Linux启动时，硬件时钟会读取系统时钟的设置，之后系统时钟就独立于硬件时钟运作  
 ```
   
```bash

 [root@node2 ~]# vi /etc/sysconfig/ntpd  
 The following item added by Robinson  
 SYNC_HWCLOCK=yes  
 OPTIONS="-x -u ntp:ntp -p /var/run/ntpd.pid"    
```
  
5、配置ntp自启动服务   
```bash

 [root@node1 ~]# systemctl enable ntpd
 [root@node2 ~]# systemctl enable ntpd
```

6、在两节点启动ntp服务  
```bash
 [root@node1 ~]# systemctl restart ntpd
 Shutting down ntpd: [FAILED]  
 [root@node1 ~]# systemctl restart ntpd
 ntpd: Synchronizing with time server: [FAILED]  
 Starting ntpd: [  OK  ]  
  
 [root@node2 ~]# service ntpd restart  
 Shutting down ntpd: [  OK  ]  
 ntpd: Synchronizing with time server: [  OK  ]  
 Syncing hardware clock to system time [  OK  ]  
 Starting ntpd: [  OK  ]    
```
  
7、查看ntp状态  
```bash
 [root@node1 ~]# ntpq -p  
      remote           refid      st t when poll reach   delay   offset  jitter  
 ==============================================================================  
  LOCAL(0)        .LOCL.          10 l   40   64    1    0.000    0.000   0.001  
    
 [root@node2 ~]# ntpq -p  
      remote           refid      st t when poll reach   delay   offset  jitter  
 ==============================================================================  
  node1.szdb.com  .INIT.          16 u   60   64    0    0.000    0.000   0.000  
  LOCAL(0)        .LOCL.          10 l   59   64    1    0.000    0.000   0.001  
```
 也可以使用watch ntpq -p方式查看实时状态  

8、ntp的相关日志  
```bash
[root@bigboy tmp]# cat /var/log/messages | grep ntpd 
```