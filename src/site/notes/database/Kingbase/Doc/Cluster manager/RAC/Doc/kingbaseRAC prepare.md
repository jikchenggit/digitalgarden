---
{"author":"aming","email":"jikcheng@163.com","title":"kingbaseRAC prepare","creation_date":"2022-10-28 14:06","Last modified date":"2022-11-25 16:00","tags":"kingbaseRAC prepare","File Folder with relative path":"database/Kingbase/Doc/Cluster manager/RAC/misc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/kingbase/doc/cluster-manager/rac/doc/kingbase-rac-prepare/","dgPassFrontmatter":true}
---



## 环境准备

| IP 地址        | Linux 版本 | kingbase 版本  | 主机名 |
| -------------- | ---------- | -------------- | ------ |
| 192.168.10.223 |            |                | VIP    |
| 192.168.10.224 | Rocky 8.5  | kingbase8.6rac | node1  |
| 192.168.10.225 | Rocky 8.5  | kingbase8.6rac | node2  |
| 192.168.10.226 |            |                | VIP2   |



## 操作系统环境配置
### node1
#### 执行一键优化Linux 脚本
```bash
./optimize_system_conf_kcp.sh
```
#### 配置安装包环境
```bash
yum install ncurses-compat-libs -y
```
#### 配置主机名称
1、设置主机名称
```bash
vi /etc/hostname
--------------------input------------------------------
node1
```
2、配置hosts 

```bash
cat >>  /etc/hosts << EOF
192.168.10.224 node1
192.168.10.225 node2
EOF
```

#### 安装数据库
```ad-warning
分别在每台机器上安装kingbase 单机版。自动生成/opt/KingbaseHA。
```
[[database/Kingbase/Doc/Initd/kingbaes 数据库安装和启停\|kingbaes 数据库安装和启停]]

#### 配置NTP时间同步

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



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

</div></div>

如果为Linux 7请参见：[[soft/Doc/NTP/Linux 7 chrony Time Synchronization Services\|Linux 7 chrony Time Synchronization Services]]。

#### 配置terminfo


通过如下命令查看 xterm 是否在默认路径 (一般为/usr/share/terminfo/x) 下


```bash
ls -l /usr/share/terminfo/x/xterm

```

如果存在，则无需额外配置。否则需要进行配置，通过如下命令查看 term 位置

```bash
find / -name xterm
ln -s /lib/terminfo/x/xterm /usr/share/terminfo/x/xterm
```

### node2
#### 执行一键优化Linux 脚本
```bash
./optimize_system_conf_kcp.sh
```
#### 配置安装包环境
```bash
yum install ncurses-compat-libs -y
```
#### 配置主机名称
1、设置主机名称
```bash
vi /etc/hostname
--------------------input------------------------------
node2
```
2、配置hosts 

```bash
cat >>  /etc/hosts << EOF
192.168.10.224 node1
192.168.10.225 node2
EOF
```

#### 安装数据库
```ad-warning
分别在每台机器上安装kingbase 单机版。自动生成/opt/KingbaseHA。
```
[[database/Kingbase/Doc/Initd/kingbaes 数据库安装和启停\|kingbaes 数据库安装和启停]]

#### 配置NTP时间同步

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



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

</div></div>

如果为Linux 7请参见：[[soft/Doc/NTP/Linux 7 chrony Time Synchronization Services\|Linux 7 chrony Time Synchronization Services]]。

#### 配置terminfo


通过如下命令查看 xterm 是否在默认路径 (一般为/usr/share/terminfo/x) 下


```bash
ls -l /usr/share/terminfo/x/xterm
```

如果存在，则无需额外配置。否则需要进行配置，通过如下命令查看 term 位置

```bash
find / -name xterm
ln -s /lib/terminfo/x/xterm /usr/share/terminfo/x/xterm
```
### 共享磁盘
#### 共享磁盘的规划
目前针对分库方案，推荐使用 n+1 个 LUN。
1、 n 为要节点的数目。
2、还有一个作为投票盘使用。一般要求投票盘的大小不小于 100MB。
#### 共享磁盘类型
1、采用 iSCSI 设备作为共享磁盘
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^au6lajbxv7i\|采用 iSCSI 设备作为共享磁盘]]
2、采用多点可见存储设备作为共享磁盘
（1）DAS 方式
（2）虚拟机共享磁盘。本文将采用此方式。
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^pd5s9a9l1ki\|采用多点可见存储设备作为共享磁盘]]

#### 创建共享磁盘

[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^pwy4qpstaxc\|虚拟机共享虚拟磁盘]]
1、采用共享虚拟磁盘方式时：磁盘的配置方式。

| 配置名称   | 推荐配置内容 |
| ---------- | ------------ |
| 类型       | 厚置备、置零 |
| 控制器位置 | SCSI 控制器1 |
| 磁盘模式   | 独立-永久    |



<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




## 创建共享磁盘
### EXSI 支持的共享磁盘
1. 创建共享磁盘，在对应添加磁盘即可。另外一台节点添加相同的磁盘文件。
2. 目前ESXI 支持厚置备置零磁盘作为共享磁盘。
![EXSI 支持的共享磁盘|400](https://www.aming.work:8084/images/2022/10/28/20221028154113.png)
### 拷贝已建好的RAC 共享磁盘

1、登录vmware EXSI 主机
2、创建共享磁盘存放的目录。
```bash
cd /vmfs/volumes/product_HDD
mkdir KRAC9_DISK
``` 
3、将对应生成好的共享磁盘放在目录中。
 ![拷贝共享磁盘|400](https://www.aming.work:8084/images/2022/10/28/20221028153729.png)
4. 完成拷贝。
![ESXI 拷贝文件|400](https://www.aming.work:8084/images/2022/10/28/20221028154349.png)

1. 添加硬盘（注意，所有节点都需要添加）
   ![向每个节点添加共享磁盘|400](https://www.aming.work:8084/images/2022/10/28/20221028154436.png)

</div></div>


#### 制作文件系统

```bash
for i in {b..k}; 
do  mkfs.xfs -f /dev/sd$i 
done
```

```ad-warning
注意不要使用LVM ，磁盘卷组信息。LVM 卷组信息其他节点无法识别。

```
---

[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbaseHA Soft Get\|kingbaseHA Soft Get]]