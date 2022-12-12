---
{"author":"aming","email":"jikcheng@163.com","title":"yum error","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"yum error","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD/yum/error","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/yum/error/yum-error/","dgPassFrontmatter":true}
---



## Yum Fatal Python error
### 报错现象:
```bash
yum update 
--------------------------------------output:-------------------------------
Fatal Python error: pycurl: libcurl link-time version is older than compile-timeversion Aborted
```
### 报错原因：
无法找到对应的lib库。
### 报错解决方法:
```bash
# 编辑
vi /etc/ld.so.conf.d/vmware-tools-libraries.conf
# 添加下面这行内容
/lib64 /usr/lib64
# 保存后，重载lib库。
ldconfig
```




##  Error: rpmdb open failed
### 报错信息：

```info
错误：rpmdb: BDB0113 Thread/process 19937/139717329233984 failed: BDB1507 Thread died in Berkeley DB library
错误：db5 错误(-30973) 来自 dbenv->failchk：BDB0087 DB_RUNRECOVERY: Fatal error, run database recovery
错误：无法使用 db5 -  (-30973) 打开 Packages 索引
错误：无法从 /var/lib/rpm 打开软件包数据库
CRITICAL:yum.main:
Error: rpmdb open failed

```

### 解决方案:
```bash
cd /var/lib/rpm      # rpmdb所在目录
ls | grep 'db.'   # 列出相关rpmdb文件__db.001__db.002__db.003__db.004
# 将原rpmdb文件都更名为结尾带.bak的文件或者
for i in $(ls | grep 'db.');do mv $i $i.bak;done
rm -f __db.*     # 清除原rpmdb文件
rpm --rebuilddb     # 重建rpm数据库
yum clean all     # 清除所有yum的缓存
```

## RPMDB altered outside of yum.
###  报错信息

```info
Warning: RPMDB altered outside of yum.
```
### 报错原因：
使用了其他yum源信息库，而后又更改了。
### 解决办法: 删除yum的历史记录
```bash
yum clean all     # 清除所有yum的缓存
```
##  RHNRHN-ORG-TRUSTED-SSL-CERT
### 报错信息

```bash
yum install http-server
--------------------------------------output:-------------------------------
Loaded plugins: langpacks, ulninfo
rhn-plugin: ERROR: can not find RHNS CA file: /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT
```
### 报错原因
CA 证书不对，需要手工恢复证书。

### 解决办法
```bash
cp /usr/share/rhn/RHNS-CA-CERT /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT 
--------------------------------------output:-------------------------------
yum install http-server
Loaded plugins: langpacks, ulninfo
```
## epel. Please verify its path and try again

### 报错信息

在CentOS 6.3 x86_64下安装php-mcrypt的时候出现了问题：
```
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again，需要安装epel源。
```

### 解决方法： 
一句话：把/etc/yum.repos.d/epel.repo，文件第3行注释去掉，把第四行注释掉。
具体如下：
打开/etc/yum.repos.d/epel.repo，将

```bash
[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
```
修改为

```bash
[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
```
##  problem making ssl connection
### 报错信息
```
problem making ssl connection
```
### 报错原因
缺少ssl证书认证本地获取的问题导致。
### 解决办法：

```bash
yum install -y ca-certificates
```
