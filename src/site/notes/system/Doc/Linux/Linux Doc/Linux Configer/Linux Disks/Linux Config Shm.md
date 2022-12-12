---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Config Shm","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Config Shm","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Configer/Linux Disks","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-configer/linux-disks/linux-config-shm/","dgPassFrontmatter":true}
---


*  调整/dev/shm
```sh
root@centos-fuwenchao mntsda3]# vi /etc/fstab
      1
      2 #
      3 # /etc/fstab
      4 # Created by anaconda on Fri Nov  1 21:18:42 2013
      5 #
      6 # Accessible filesystems, by reference, are maintained under '/dev/disk'
      7 # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
      8 #
      9 UUID=8e319772-a274-4031-a53f-1178b3ab4de6 /                         ext4    defaults        1 1
     10 UUID=ad4de750-9575-4040-a403-08c0642f0f2c swap                    swap    defaults        0 0
     11 tmpfs                   /dev/shm                tmpfs   defaults,size=2048M        0 0
     12 devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
     13 sysfs                   /sys                    sysfs   defaults        0 0
     14 proc                    /proc                   proc    defaults        0 0
```
* 重新挂载
```sh
mount -o remount /dev/shm
```
