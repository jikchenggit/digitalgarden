---
{"author":"aming","email":"jikcheng@163.com","title":"yum source  config","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"yum source  config","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD/yum/技巧点","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/yum//yum-source-config/","dgPassFrontmatter":true}
---


# Centos 7.2 本地源
```sh
yum clean all 
yum install 需要的包
mkdir /rpm
find /var -name *.rpm -exec cp rfp {} /rpm \;
[root@iZ88gv3enkvZ yum.repos.d]# ls /rpm
compat-libstdc++-33-3.2.3-69.el6.x86_64.rpm  libstdc++-4.4.7-23.el6.x86_64.rpm
[root@iZ88gv3enkvZ yum.repos.d]# cd /rpm/
[root@iZ88gv3enkvZ rpm]# createrepo ./
Spawning worker 0 with 2 pkgs
Workers Finished
Gathering worker results

Saving Primary metadata
Saving file lists metadata
Saving other metadata
Generating sqlite DBs
Sqlite DBs complete
[root@iZ88gv3enkvZ rpm]# 
```

#  yum 配置光盘镜像源
* 配置yum源

```sh
cat >  /etc/yum.repos.d/iso.repo <<eof
[iso]
name=Yum Source
baseurl=file:///mnt/
enabled=1
gpgcheck=0
eof
```
* 移除原有
```sh
mv public-yum-ol7.repo public-yum-ol6.repo_bak
mv Oracle\ Linux\ 7\ Update\ 1\\,\ x86\ 64\ bit.iso oracle\_linux\_7_1.iso
```
* 创建挂载脚本
```sh
cat > /etc/yum.repos.d/mount.sh << eof
mount -o loop -t iso9660 /app/OEL_7.5.iso /mnt
eof
```
* 权限
```sh
chmod +x /etc/yum.repos.d/mount.sh
```

* 创建缓存
```sh
yum makecache
```

* 配置文件 `vi /etc/yum.repos.d/iso.repo`

```sh
[iso]
name=Yum Source
baseurl=file:///mnt/
enabled=1
gpgcheck=0
cd /etc/yum.repos.d/
mv packagekit-media.repo packagekit-media.repo_bak
mv public-yum-ol6.repo public-yum-ol6.repo_bak
```

* 挂载iso

```sh
mount -o loop -t iso9660 /app/setup/oracle\_linux\_7_1.iso /media
```

* 建立缓存

```sh
yum makecache
```


# Centos 7.2 源 

```
[ol7_latest]
name=Oracle Linux $releasever Latest ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/latest/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_u0_base]
name=Oracle Linux $releasever GA installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/0/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_u1_base]
name=Oracle Linux $releasever Update 1 installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/1/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_u2_base]
name=Oracle Linux $releasever Update 2 installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/2/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_u3_base]
name=Oracle Linux $releasever Update 3 installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/3/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_u4_base]
name=Oracle Linux $releasever Update 4 installation media copy ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/4/base/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_UEKR4]
name=Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux $releasever ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/UEKR4/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1

[ol7_UEKR3]
name=Latest Unbreakable Enterprise Kernel Release 3 for Oracle Linux $releasever ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/UEKR3/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_optional_latest]
name=Oracle Linux $releasever Optional Latest ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/optional/latest/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_UEKR3_OFED20]
name=OFED supporting tool packages for Unbreakable Enterprise Kernel on Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/UEKR3_OFED20/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
priority=20

[ol7_UEKR4_OFED]
name=OFED supporting tool packages for Unbreakable Enterprise Kernel Release 4 on Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/UEKR4/OFED/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
priority=20

[ol7_MySQL57]
name=MySQL 5.7 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/MySQL57_community/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_MySQL56]
name=MySQL 5.6 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/MySQL56/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_MySQL55]
name=MySQL 5.5 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/MySQL55/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_openstack30]
name=OpenStack 3.0 packages for Oracle Linux 7 (x86_64)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/openstack30/x86_64/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_openstack_extras]
name=OpenStack 3.0 Extra packages for Oracle Linux 7 (x86_64)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/openstack_extras/x86_64/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_openstack21]
name=OpenStack 2.1 packages for Oracle Linux 7 (x86_64)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/openstack21/x86_64/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
priority=20
enabled=0

[ol7_openstack20]
name=OpenStack 2.0 packages for Oracle Linux 7 (x86_64)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/openstack20/x86_64/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
priority=20
enabled=0

[ol7_ceph]
name=Ceph Storage for Oracle Linux Release 2.0 - Oracle Linux 7.2 or later ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/ceph/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_ceph10]
name=Ceph Storage for Oracle Linux Release 1.0 - Oracle Linux 7.1 or later ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/ceph10/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_software_collections]
name=Software Collection Library release 2.3 packages for Oracle Linux 7 (x86_64)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/SoftwareCollections/x86_64/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_spacewalk24_server]
name=Spacewalk Server 2.4 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/spacewalk24/server/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_spacewalk24_client]
name=Spacewalk Client 2.4 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/spacewalk24/client/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_spacewalk26_server]
name=Spacewalk Server 2.6 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/spacewalk26/server/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_spacewalk26_client]
name=Spacewalk Client 2.6 for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/spacewalk26/client/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0

[ol7_MODRHCK]
name=Latest RHCK with fixes from Oracle for Oracle Linux $releasever ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/MODRHCK/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
priority=20
enabled=0
```