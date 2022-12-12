---
{"author":"aming","email":"jikcheng@163.com","title":"Linux PV,VG,LV EXT File system","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux PV,VG,LV EXT File system","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Configer/Linux Disks/LVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-configer/linux-disks/lvm/linux-pv-vg-lv-ext-file-system/","dgPassFrontmatter":true}
---


# 1. PV,VG,LV （EXT 文件系统）
* 创建磁盘分区
```sh
fdisk /dev/sdb 
```
## 1.1. 检查磁盘类型
* 需要LVM
```sh
fdisk -l 
```
* 准备物理卷
```sh
pvcreate /dev/sdb1
pvcreate /dev/sdb2
pvcreate /dev/sdb3
``` 
* 物理卷显示
```sh
pvdisplay 
```
* 删除物理卷
```sh
pvremove /dev/sdb1
```
## 1.2. 创建VG
```sh
vgcreate volume-group1 /dev/sdb1 /dev/sdb2 /dev/sdb3
```
* 显示VG
```sh
vgdisplay
```
## 1.3. 删除VG
```sh
vgremove volume-group1 
```
## 1.4. 扩展VG
* 新创建一个PV
```sh
pvcreate /dev/sdc2
```
* 增加PV 到VG 上去
 ```sh
vgextend volume-group1 /dev/sdc2
```
* 减少VG（从VG 里删除PV）
```sh
vgreduce volume-group1 /dev/sdc2
```


## 1.5. 创建LV
```sh
lvcreate -L 100M -n lv1 volume-group1 
```
* 显示LV
```sh
lvdisplay 
```
* 制作文件系统
```sh
mkfs.ext4 /dev/volume-group1/dsg
mkdir /dsg
mount /dev/volume-group1/dsg /dsg
```

* 扩展LV
```sh
umount /dsg
```
* 设置卷为200m
```sh
lvresize -L 200M /dev/volume-group1/dsg
```
* 扩展操作系统
```sh
resize2fs /dev/volume-group1/dsg
```
* 查看lv 是否扩展
```sh
lvdisplay 
```
## 1.6. 缩减LV
* 卸载卷
```sh
umount /dev/volume-group1/dsg
```
* 检查磁盘错误
```sh
e2fsck -f /dev/volume-group1/dsg
```
* 缩小文件系统
```sh
resize2fs /dev/volume-group1/lv1 100M 
```
* 缩小LV
```sh
lvresize -L 100M /dev/volume-group1/lv1 
```
* 查看LV 是否缩小
```sh
lvdisplay 
```