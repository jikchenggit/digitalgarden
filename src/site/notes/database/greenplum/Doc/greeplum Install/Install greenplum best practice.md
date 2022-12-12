---
{"author":"aming","email":"jikcheng@163.com","title":"Install greenplum best practice","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Install greenplum best practice","File Folder with relative path":"database/greenplum/Doc/greeplum Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/greenplum/doc/greeplum-install/install-greenplum-best-practice/","dgPassFrontmatter":true}
---


# 安装greenplum 最佳实践
## 操作系统硬件配置
### 配置磁盘
```bash
pvcreate /dev/vdc
vgcreate  greenplum /dev/vdc
lvcreate -l 1310719 -n gp greenplum
mkfs.xfs  /dev/mapper/greenplum-gp
mount /dev/mapper/greenplum-gp  /opt/
```

```bash
cat >> /etc/fstab << EOF
/dev/mapper/greenplum-gp  /opt                  xfs nodev,noatime,nobarrier,inode64 0 0
EOF
```
### 配置hosts
```bash
cat >> /etc/hosts << EOF
23.123.253.63 master
23.123.253.64 standby
23.123.253.65 node1
23.123.253.66 node2
EOF
```
### 禁用防火墙
```bash
getenforce -l
```
### 配置yum 源
```bash
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
cat > /etc/yum.repos.d/CentOS-Base.repo << EOF
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
#mirrorlist=10.31.145.40?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://10.8.141.88/centos/\$releasever/os/\$basearch/
gpgcheck=1
gpgkey=http://10.8.141.88/centos/RPM-GPG-KEY-CentOS-7

#released updates 
#[updates]
#name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
#gpgcheck=1
#gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
#[extras]
#name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
#gpgcheck=1
#gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
#[centosplus]
#name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
#gpgcheck=1
#enabled=0
#gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
EOF
```
### 安装依赖包
``` 
yum install apr apr-util bash bzip2 curl krb5 libcurl libevent libxml2 libyaml zlib openldap openssh openssl openssl-libs  perl readline rsync R sed  tar zip chrony net-tools -y
``` 
```
yum install -y java
```


### 禁用selinux
```bash
vi /etc/selinux/config
```
### 禁用防火墙
```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```
### 确认selinux 关闭
```bash
sestatus
```
### 配置内核参数列表
```bash
cat >> /etc/sysctl.conf << EOF
# kernel.shmall = _PHYS_PAGES / 2 # See Shared Memory Pages
kernel.shmall = 8223627
# kernel.shmmax = kernel.shmall * PAGE_SIZE
kernel.shmmax = 33683976192
kernel.shmmni = 4096
vm.overcommit_memory = 2 # See Segment Host Memory
vm.overcommit_ratio = 95 # See Segment Host Memory

net.ipv4.ip_local_port_range = 10000 65535 # See Port Settings
kernel.sem = 500 2048000 200 4096
kernel.sysrq = 1
kernel.core_uses_pid = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.msgmni = 2048
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.tcp_max_syn_backlog = 4096
net.ipv4.conf.all.arp_filter = 1
net.core.netdev_max_backlog = 10000
net.core.rmem_max = 2097152
net.core.wmem_max = 2097152
vm.swappiness = 10
vm.zone_reclaim_mode = 0
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.dirty_background_ratio = 3
vm.dirty_ratio = 10
vm.min_free_kbytes = 1973670
EOF
```
生效参数
```bash
sysctl -p
```
### 修改磁盘预读值
```bash
/sbin/blockdev --getra  /dev/mapper/greenplum-gp
/sbin/blockdev --setra 16384 /dev/mapper/greenplum-gp
```

### 磁盘预读
```bash
cat >> /etc/rc.local << EOF
/sbin/blockdev --setra 16384 /dev/mapper/greenplum-gp
EOF
```

### 更改权限
```bash
chmod +x /etc/rc.local
```
### 关闭透明巨页
1. 变更透明巨页
```bash
grubby --update-kernel=ALL --args="transparent_hugepage=never"
```

2. 重启主机
```bash
reboot
```
3.  验证是否成功
```bash
cat /sys/kernel/mm/*transparent_hugepage/enabled
always [never]
```

### 关闭RemoveIPC
```bash
cat >> /etc/systemd/logind.conf << EOF
RemoveIPC=no
EOF
service systemd-logind restart
```

## sshd配置
```bash
cat >>  /etc/ssh/sshd_config  << EOF
MaxStartups 200
MaxSessions 200
EOF
```
## NTP 服务
###  NTP 服务端
1. 修改`/etc/chrony.conf`
```bash
server 23.123.253.63 prefer
allow 23.123.253.0/24
```
2. 重启服务
```bash
systemctl  start chronyd
systemctl  status chronyd
systemctl enable chronyd
```
### NTP 客户端
1. 修改`/etc/chrony.conf`

```bash

server 23.123.253.63 iburst
```
2. 重启服务
```bash
systemctl  start chronyd
systemctl  status chronyd
```

### 验证时间同步.
```bash
chronyc sources
chronyc sourcestats
```
## 创建用户
```bash
groupadd gpadmin
useradd gpadmin -r -m -g gpadmin
 passwd gpadmin

```

## 创建密钥
```bash
su - gpadmin
ssh-keygen -t rsa -b 4096
```
## 加入sudo 组.
```bash
visudo
%wheel  ALL=(ALL)       NOPASSWD: ALL
 usermod -aG wheel gpadmin
```
## 设置环境变量
```bash
cat >> ~/.bash_profile << EOF source /usr/local/greenplum-db/greenplum_path.sh EOF
```

# 附录:
## 磁盘挂载错误修复
```bash
mount -o remount,rw /
```

