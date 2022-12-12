---
{"author":"aming","email":"jikcheng@163.com","title":"Linux fstab","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux fstab","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Configer/Linux Disks","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-configer/linux-disks/linux-fstab/","dgPassFrontmatter":true}
---


#  开机自动挂载磁盘
```sh
[root@db1 /]# vi /etc/fstab
 2. This file is edited by fstab-sync - see 'man fstab-sync' for details
LABEL=/ / ext3 defaults 1 1
none /dev/pts devpts gid=5,mode=620 0 0
none /dev/shm tmpfs defaults 0 0
none /proc proc defaults 0 0
none /sys sysfs defaults 0 0
LABEL=SWAP-sda2 swap swap defaults 0 0
/dev/sdb1 /u01 ext3 defaults 0 0
/dev/hdc /media/cdrom auto pamconsole,exec,noauto,managed 0 0
/dev/fd0 /media/floppy auto pamconsole,exec,noauto,m
```