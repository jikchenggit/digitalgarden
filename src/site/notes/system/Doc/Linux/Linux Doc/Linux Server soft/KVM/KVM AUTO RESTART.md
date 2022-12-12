---
{"author":"aming","email":"jikcheng@163.com","title":"KVM AUTO RESTART","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"KVM AUTO RESTART","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/KVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/kvm/kvm-auto-restart/","dgPassFrontmatter":true}
---


图形化界面
必须在虚拟机关机情况下完成！！！
在KVM图形化管理工具中设置开机自启动
在Startvirtual machine on host boot up 前勾选即可
命令行界面
virsh autostart 虚拟机名  :设置随宿主机开机自启动
检查在/etc/libvirt/qemu/autostart/下会生成一个（虚拟机名.xml）文件
virsh autostart --disable 虚拟机名 :取消随宿主机开机自启动
