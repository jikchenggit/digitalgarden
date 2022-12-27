---
{"author":"aming","email":"jikcheng@163.com","title":"OEL 7.5 Install DB2v11.1","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:05","tags":"OEL 7.5 Install DB2v11.1","File Folder with relative path":"database/DB2/Doc/Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/db-2/doc/install/oel-7-5-install-db-2v11-1/","dgPassFrontmatter":true}
---


## OS 准备

### 磁盘规划
- 位置规划 
```sh
/dev/mapper/ol-db2home  200G   33M  200G   1% /db2home
```



> [!warning]+ 注意
>  - 交换空间大多数系统的合理最低交换/调页空间配置为 RAM 的 25-50%
>   一般 超过16 G 配16G 。小于16G 配 内存同样大小
> - 大小规划
> ` /db2home`安装文件大小为1.5G 。/db2home 至少1.5 G 
> `/tmp` 目录需要2G 



### 内存规划
 - db2 安装至少需要1G 内存
 - 交换空间大多数系统的合理最低交换/调页空间配置为 RAM 的 25-50%
 - 启用大页支持
 - 数据库一半为自调整内存
 - 

/var 需要512M.
> [!warning]
> 安装操作系统时就需要规划好，数据库安放在何处，后期调整有点麻烦。
     
### root 和非root 用户安装的差别
| 条件 | root 用户安装 | 非 root 用户安装 |
| --- | --- | --- |
| 用户可以选择安装目录 | 是 | 否。Db2 数据库产品安装在用户的主目录中。 |
| 允许的 Db2 实例数 | 多个 | 一个 |
| 安装期间部署的文件 | 仅程序文件。您必须在完成安装后才创建实例。 | 程序文件和实例文件。Db2 数据库产品已准备就绪，可在完成安装后立即使用。 |
| 升级版本和实例 | 否 | 在安装新版本之前，不需要卸载旧版本。安装新版本，同时升级该实例。 | 
### 2.4. 内核参数设置
> root 用户安装则不需要调整内核，安装时自动会调整

| IPC 内核参数 | 最低增强设置 |
| --- | --- |
| kernel.shmmni (SHMMNI) | 256 * <RAM 大小，以 GB 计> |
| kernel.shmmax (SHMMAX) | <RAM 大小，以字节计>^1^ |
| kernel.shmall (SHMALL) | 2* <缺省系统页中的 RAM 大小 >^2^ |
| kernel.sem (SEMMNI) | 256 * <RAM 大小，以 GB 计> |
| kernel.sem (SEMMSL) | 250 |
| kernel.sem (SEMMNS) | 256 000 |
| kernel.sem (SEMOPM) | 32 |
| kernel.msgmni (MSGMNI) | 1 024 * <RAM 大小，以 GB 计> |
| kernel.msgmax (MSGMAX) | 65 536 |
| kernel.msgmnb (MSGMNB) | 65 536 ^3^ |

> 
- 在 32 位 Linux 操作系统上，SHMMAX 的最低增强设置限制为 4,294, 967,295 个字节。
 - SHMALL 限制在系统上可分配的虚拟共享内存总量。每个 Db2® 数据服务器有效地管理其使用的系统内存量（也称为已落实内存）。Db2 数据服务器会分配比其落实的内存更大的虚拟内存，以支持内存预分配和动态内存管理。内存预分配可提高性能。动态内存管理是增加或减少单独虚拟共享内存区域中的真实内存使用的过程。为了有效地支持内存预分配和动态内存管理，数据服务器常常必须在系统上分配大于物理 RAM 量的虚拟共享内存。 内核需要此值作为页数。
- 负载性能可从较大的消息队列大小（由 MSGMNB 指定，以字节计）限制获益。您可以通过运行 ipcs -q 命令查看消息队列使用情况。如果在装入操作期间，消息队列达到或接近容量，请考虑增大消息队列大小限制的字节数。
- 例子

```sh
# 3. ipcs -l

      ------ Shared Memory Limits --------
      max number of segments = 4096               // SHMMNI	
      max seg size (kbytes) = 32768               // SHMMAX
      max total shared memory (kbytes) = 8388608  // SHMALL
      min seg size (bytes) = 1

      ------ Semaphore Limits --------
      max number of arrays = 1024                 // SEMMNI
      max semaphores per array = 250              // SEMMSL
      max semaphores system wide = 256000         // SEMMNS
      max ops per semop call = 32                 // SEMOPM
      semaphore max value = 32767

      ------ Messages: Limits --------
      max queues system wide = 1024               // MSGMNI
      max size of message (bytes) = 65536         // MSGMAX
      default max size of queue (bytes) = 65536    // MSGMNB
```


```sh
#Example for a computer with 16GB of RAM:
kernel.shmmni=4096
kernel.shmmax=17179869184
kernel.shmall=8388608
#kernel.sem=<SEMMSL> <SEMMNS> <SEMOPM> <SEMMNI>
kernel.sem=250 1024000 32 4096
kernel.msgmni=16384
kernel.msgmax=65536
kernel.msgmnb=65536

#Example for a computer with 8G of RAM: kernel.shmmni=2048
kernel.shmmax=8589934592
kernel.shmall=8388608
#kernel.sem=<SEMMSL> <SEMMNS> <SEMOPM> <SEMMNI>
kernel.sem=250 1024000 32 2048
kernel.msgmni=8192
kernel.msgmax=65536
kernel.msgmnb=65536
```
## 2.5.  操作系统用户创建
- 集群用户创建

| 必需的用户 | 用户名 | 组名 |
| --- | --- | --- |
| 实例所有者 | db2inst1 | db2iadm1 |
| 受防护用户 | db2fenc1 | db2fadm1 |
| Db2 管理服务器用户 | dasusr1 | dasadm1 |

```sh
groupadd -g 1000 db2iadm1
groupadd -g 1001 db2fsdm1
groupadd -g 1002 dasadm1
```
-  创建操作系统用户
```sh
useradd -u 1004 -g db2iadm1 -m -d /home/db2inst1 db2inst1
useradd -u 1003 -g db2fsdm1 -m -d /home/db2fenc1 db2fenc1 
useradd -u 1002 -g dasadm1 -m -d /home/dasusr1 dasusr1
```

```sh
passwd db2inst1
passwd db2fenc1
passwd dasusr1
```
- 用户限制

| 硬 ulimit 资源 | 描述 | 最小值 | 建议使用的值 | 用于查询值的命令 |
| --- | --- | --- | --- | --- |
| data | 允许用于进程的最大专用内存 | 计算机中可用的内存量 | 无限制 | ulimit -Hd |
| nofile | 允许用于进程的最大打开文件数 | 大于实例中所有数据库的所有 MAXFILOP 数据库配置参数的总和 | 65536 | ulimit -Hn |
| fsize | 允许的最大文件大小 | 无限制 | 无限制 | ulimit -Hf |
修改用户限制
```sh
vi /etc/security/limits.conf 
```
```sh
db2inst1 soft    nofile          65536
db2inst1 hard    nofile          65536
db2fenc1 soft    nofile          65536
db2fenc1 hard    nofile          65536
dasusr1  soft    nofile          65536
dasusr1  hard    nofile          65536
```
- 安装需要的包
```sh
yum install libibcm libibverbs librdmacm rdma dapl ibacm ibsim  ibutils libcxgb3 libibmad libipathverbs libmlx4 libmlx5 libmthca libnes rds-tools  libstdc++  libstdc++*i686 glibc glibc*i686 gcc-c++ gcc kernel kernel-devel kernel-headers kernel-firmware ntp ntpdate sg3_utils sg3_utils-libs binutils binutils-devel m4 openssh cpp ksh libgcc file libgomp make patch perl-Sys-Syslog libpam.so.0 -y
```
# 3. 安装数据库
## 3.1. 预检查
```sh
./db2prereqcheck -v 11.1.0.0  | more
```
> 请注意not matched 条目
## 3.2. 上传软件解压软件
## 3.3. 开始安装
```sh
./db2setup 
```
- 初始界面



- 选择需要安装的组件



- 选择新添加的语言

- 数据库监听创建

- 是否提供加密组件


 - text 搜索组件端口

 - 是否保存到响应文件

- 开始安装


# 4. 安装后测试
- 切换到实例拥有者
```sh
su - db2inst1
db2start
```
- 创建sample 数据库
```sh
db2sampl
```
- 连接到数据库查询相关表
```sh
db2
```
 - 查询
```sql
connect to sample
select * from staff where dept = 20
connect reset
```