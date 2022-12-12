---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Make (PV,VG,LV)XFS file system","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Make (PV,VG,LV)XFS file system","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Configer/Linux Disks/LVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-configer/linux-disks/lvm/linux-make-pv-vg-lv-xfs-file-system/","dgPassFrontmatter":true}
---


# 1. 添加新磁盘(PV,VG,LV)XFS文件系统
* 添加磁盘后扫描scsi

```sh
ls /sys/class/scsi_host/
echo "- - -" > /sys/class/scsi_host/host0/scan
echo "- - -" > /sys/class/scsi_host/host1/scan
echo "- - -" > /sys/class/scsi_host/host2/scan
```


```sh
ls /sys/class/scsi_device/
echo 1 > /sys/class/scsi_device/1\:0\:0\:0/device/rescan
echo 1 > /sys/class/scsi_device/2\:0\:0\:0/device/rescan
```
* 创建磁盘分区并指定LINUX LVM 磁盘卷组
> 注意分区类型为8e

* 更改磁盘为LVM

```sh
[root@OEL7 ~]# fdisk /dev/sdb 
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。


命令(输入 m 获取帮助)：t
分区号 (1,2，默认 2)：
Hex 代码(输入 L 列出所有代码)：8e
已将分区“Linux”的类型更改为“Linux LVM”

命令(输入 m 获取帮助)：t
分区号 (1,2，默认 2)：1
Hex 代码(输入 L 列出所有代码)：8e
已将分区“Linux”的类型更改为“Linux LVM”

命令(输入 m 获取帮助)：w
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。

====================
```
## 1.1. 创建物理卷(PV)

```sh
[root@OEL7 ~]# pvcreate /dev/sdc1
  Physical volume "/dev/sdc1" successfully created
[root@OEL7 ~]# pvcreate /dev/sdc2
  Physical volume "/dev/sdc2" successfully created
[root@OEL7 ~]# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created
[root@OEL7 ~]# pvcreate /dev/sdb2
  Physical volume "/dev/sdb2" successfully created
```

* 查看物理卷信息和卷组信息(PV)
```sh
pvdisplay
```
* 删除物理卷(PV)
```sh
pvremove  /dev/sdb1
```


## 1.2. 创建卷组（VG）
```sh
vgcreate volume-group1 /dev/sdb1 /dev/sdb2 /dev/sdc1
```
* 显示卷组(VG)

```sh
vgdisplay
```
* 删除卷组(VG)
```sh
vgremove volume-group1
```
## 1.3. 扩展VG
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



## 1.4. 创建LV
```sh
lvcreate -L 20G -n DSG volume-group1 
```
* 删除LV
```sh
lvremove volume-group1
```
* 显示LV
```sh
lvdisplay
```
* 制作文件系统

```sh
mkfs.xfs /dev/volume-group1/DSG
```

* 挂载目录
```sh
mkdir /dsg
mount /dev/volume-group1/DSG /dsg
```
* 开启自启动
```sh
vi /etc/fstab
/dev/volume-group1/DSG  /dsg                    xfs     defaults        0 0
```
* 扩展LV 

```sh
lvresize -L  25G /dev/volume-group1/DSG
```

* 扩展LV 文件系统
```sh
xfs_growfs  /dev/volume-group1/DSG
```

* 缩小LV
缩小文件系统
注意：XFS 系统智能增大不能缩小。以下使用ext 文件系统。
```sh
xfs_admin: 调整 xfs 文件系统的各种参数  
xfs_copy: 拷贝 xfs 文件系统的内容到一个或多个目标系统（并行方式）  
xfs_db: 调试或检测 xfs 文件系统（查看文件系统碎片等）  
xfs_check: 检测 xfs 文件系统的完整性  
xfs_bmap: 查看一个文件的块映射  
xfs_repair: 尝试修复受损的 xfs 文件系统  
xfs_fsr: 碎片整理  
xfs_quota: 管理 xfs 文件系统的磁盘配额  
xfs_metadump: 将 xfs 文件系统的元数据 (metadata) 拷贝到一个文件中  
xfs_mdrestore: 从一个文件中将元数据 (metadata) 恢复到 xfs 文件系统  
xfs_growfs: 调整一个 xfs 文件系统大小（只能扩展）  
xfs_logprint: 打印xfs文件系统的日志  
xfs_mkfile: 创建xfs文件系统  
xfs_info: 查询文件系统详细信息  
xfs_ncheck: generate pathnames from i-numbers for XFS  
xfs_rtcp: XFS实时拷贝命令   
xfs_io: 调试xfs I/O路径
```