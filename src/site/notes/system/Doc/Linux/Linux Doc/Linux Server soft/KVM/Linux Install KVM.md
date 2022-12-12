---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Install KVM","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Install KVM","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/KVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/kvm/linux-install-kvm/","dgPassFrontmatter":true}
---


# 1. 安装操作系统组件

```sh
yum  groupinstall "DevelopmentTools"
yum  groupinstall "Virtualization" "Virtualization Client""Virtualization Platform"
```
把 root 用户增加到KVM 组
```sh
usermod root -g root -G kvm
```


## 1.1. Centos6.4 64bit 安装KVM    

 ```sh
[root@test01 ~]#egrep 'vmx|svm' /proc/cpuinfo//
```
>  首先确定你的cpu是否支持vmx或者svm虚拟化,vmx属于inter处理器,svm属于amd处理器，或者用cpu-z查看你的处理器是否支持vt-x虚拟化，bios中开启vt支持。

    要安装虚拟化要有下面4个包才能做支撑
        @virtualization //提供虚拟机的环境，主要包含qumu-kvm

        @virtualization-client//管理和安装虚拟机实例的客户端，主要有python-virtinst,virt-manager,virt-viewer

        @virtualization-platform//提供访问和控制虚拟客户端的接口，主要有libvirt，libvirt-client

        @virtualization-tools //管理离线虚拟机镜像的工具，主要有libguestfs根据需求选择软件包，一般都安装1，2，3 利用yum groupinstall "Virtualization" "Virtualization Client" "Virtualization Platform"
