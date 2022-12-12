---
{"author":"aming","email":"jikcheng@163.com","title":"KADB Install best pracitce","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 18:43","tags":"KADB Install best pracitce","File Folder with relative path":"database/KADB","remark":null,"other":null,"dg-publish":true,"permalink":"/database/kadb/kadb-install-best-pracitce/","dgPassFrontmatter":true}
---


## 操作系统配置
### 调优操作系统（所有节点）
```bash
sh optimize_system_conf_kcp.sh
```
### 安装依赖包（所有节点）
```bash
yum -y install bash json-c openssh openssh-clients perl sed sysstat tar vim-minimal zip xfsprogs zlib
```

### 其他（所有节点）
1. 本地DNS 配置
```bash
cat >> /etc/hosts <<EOF
192.168.10.200 master
192.168.10.201 standby
192.168.10.202 node1
192.168.10.203 node2
192.168.10.204 node3
192.168.10.205 node4
EOF
```
2. 避免SSH连接前提示
```bash
cat >> /etc/ssh/ssh_config <<EOF
StrictHostKeyChecking no
EOF
```
3. 加速SSH连接
```bash
sed -i 's/^GSS/#&/g' /etc/ssh/sshd_config; service sshd reload;
```
3. 修改磁盘预读大小
```bash
/sbin/blockdev --setra 16384  /dev/vda
/sbin/blockdev --getra /dev/vda
echo deadline > /sys/block/vda/queue/scheduler
```
### 创建用户（所有节点）
```bash
useradd mppadmin;
echo "kingbase"|passwd --stdin mppadmin
```
### 上传软件（所有节点）

```bash
[mppadmin@nodex]$ ls /home/mppadmin/
KADB-V003R002C001B0110-CENTOS7-x86_64.run  license.dat
chown mppadmin:mppadmin /home/mppadmin/*
```

### 设置环境变量（master，standby）

```bash
cat >> /home/mppadmin/.bashrc <<EOF
source /home/mppadmin/MPP/mpp_path.sh
export MASTER_DATA_DIRECTORY=/home/mppadmin/dbdata/master/mppseg-1
EOF
```



## 安装MPP
### 解压软件
> 所有节点
```bash
su - mppadmin
bash ./KingbaseAnalyticsDB-V003R002C001B0110-CENTOS7-x86_64.run /home/mppadmin/MPP/
```
### 创建主机配置文件（master节点）
```bash
cat >> /home/mppadmin/hostfile <<EOF
master
standby
node1
node2
node3
node4
EOF
```

### 主库上执行：交换公钥（master节点）
```bash
gpssh-exkeys -f /home/mppadmin/hostfile
```
### 复制license 文件到所有节点（master 节点）
```bash
gpssh -f /home/mppadmin/hostfile -e 'cp /home/mppadmin/license.dat  /home/mppadmin/MPP/bin'
```
### 创建目录（master节点）
```bash
#为主备管理节点（node1和node2)创建master备份目录
  gpssh -h master -e "mkdir /home/mppadmin/master_backup"
  gpssh -h standby -e "mkdir /home/mppadmin/master_backup"
#为主备管理节点（node1和node2)创建master实例目录
  gpssh -h master -e 'mkdir -p /home/mppadmin/dbdata/master'
  gpssh -h standby -e 'mkdir -p /home/mppadmin/dbdata/master'
#为所有segment实例创建数据目录/dbdata，备份目录/dbbackup
  gpssh -f /home/mppadmin/hostfile -e 'mkdir -p /home/mppadmin/dbdata'
  gpssh -f /home/mppadmin/hostfile -e 'mkdir -p /home/mppadmin/dbbackup'
#为所有segment实例创建primary和mirror目录
  gpssh -f /home/mppadmin/hostfile -e 'mkdir -p /home/mppadmin/dbdata/primary'
  gpssh -f /home/mppadmin/hostfile -e "mkdir -p /home/mppadmin/dbdata/mirror"
```
### 测试（master 节点）
1. 测试网络
```bash
gpcheckperf -f /home/mppadmin/hostfile -r N -D -d /home/mppadmin/dbdata
```
2. 测试磁盘
```bash
gpcheckperf -f /home/mppadmin/hostfile -d /home/mppadmin/dbdata -r d -D -v
```

## 初始化数据库
1. 配置参数（master）
```bash
cat >> /home/mppadmin/initsystem_config <<EOF
ARRAY_NAME="MPP"
#segment节点名prefix
SEG_PREFIX=mppseg
#segment节点基础端口，每多一个segment会自动增加1，默认为40000
PORT_BASE=40000
#用于申明segment目录及每节点中segment数量，多个目录用空格隔开，下面示例中为2个目录，暨每台主机2个segment放在相同的目录下
declare -a DATA_DIRECTORY=(/home/mppadmin/dbdata/primary /home/mppadmin/dbdata/primary)
#master主机地址
MASTER_HOSTNAME=master
#master目录
MASTER_DIRECTORY=/home/mppadmin/dbdata/master
#master端口，默认为5432
MASTER_PORT=5432
TRUSTED_SHELL=ssh
CHECK_POINT_SEGMENTS=8
ENCODING=UNICODE
#选择需要初始化segment的主机名，列出到hostfile目录
MACHINE_LIST_FILE=/home/mppadmin/hostfile
#mirror segment起始端口号
MIRROR_PORT_BASE=43000
#primary segment主备同步的起始端口号
REPLICATION_PORT_BASE=34000
#mirror segment主备同步的起始端口号
MIRROR_REPLICATION_PORT_BASE=44000
#Mirro segment的数据目录，与primary segment目录数量要对应
declare -a MIRROR_DATA_DIRECTORY=(/home/mppadmin/dbdata/mirror /home/mppadmin/dbdata/mirror)
EOF
```
2. 初始化集群
```bash
gpinitsystem -c /home/mppadmin/initsystem_config -s standby
```



# 实践
```bash
[proxy]
192.168.10.171
[app]
192.168.10.168
192.168.10.169
[nosql]
192.168.10.170
[db]
192.168.10.167
```

## 安装NGinx
```bash
 ansible proxy -m yum -a "name=nginx state=present"
 ```
## 验证NGINX 安装是否成功
```bash
ansible proxy -m command -a "nginx -v"
```


## 安装本地rpm 包
```bash
ansible proxy -m yum -a "name=/usr/local/src/nginx-release-centos-6-0.el6.ngx.noarch.rpm state=present"
```