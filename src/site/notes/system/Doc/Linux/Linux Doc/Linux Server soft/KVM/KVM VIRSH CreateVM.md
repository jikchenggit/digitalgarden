---
{"author":"aming","email":"jikcheng@163.com","title":"KVM VIRSH CreateVM","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"KVM VIRSH CreateVM","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/KVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/kvm/kvm-virsh-create-vm/","dgPassFrontmatter":true}
---


# 1. 使用VIRSH 创建虚拟机
1. Uuid 生成
```sh
[root@KVM dumpxml]# UUID=`uuidgen`
[root@KVM dumpxml]# cp dbserver.xml templacte.xml
[root@KVM dumpxml]# sed -i "s,%UUID%,$UUID,g" my_cao.xml
```
1. Image
```sh
[root@KVM kvm]# qemu-img create -f raw dbserver11.2.0.4.raw 30G
Formatting 'dbserver11.2.0.4.raw', fmt=raw size=32212254720 
[root@KVM kvm]# sed -i "s,%IMAGE_PATH%,/kvm/dbserver11.2.0.4.raw,g" my_cao.xml
```
3. 设置虚拟光盘

```sh
sed -i "s,%RAW_ISO_PATH%,/app/os/Oracle_Linux_Release_7_Update_2_for_x86_64.iso,g" dbserver1.xml
```
4. 设置桥接网卡
```sh
[root@KVM kvm]# MAC="fa:92:$(dd if=/dev/urandom count=1 2>/dev/null | md5sum | sed 's/^\(..\)\(..\)\(..\)\(..\).*$/\1:\2:\3:\4/')";
[root@KVM kvm]# sed -i "s,%MAC%,$MAC,g" dbserver1.xml
```

5. 设置虚拟网卡mac
```sh
[root@KVM kvm]# MAC="52:54:$(dd if=/dev/urandom count=1 2>/dev/null | md5sum | sed 's/^\(..\)\(..\)\(..\)\(..\).*$/\1:\2:\3:\4/')";
[root@KVM kvm]# sed -i "s,%MAC2%,$MAC2,g" my_cao.xml
```
