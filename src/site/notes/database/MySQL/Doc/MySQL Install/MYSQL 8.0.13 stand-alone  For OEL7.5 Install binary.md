---
{"author":"aming","email":"jikcheng@163.com","title":"MYSQL 8.0.13 stand-alone  For OEL7.5 Install binary","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"MYSQL 8.0.13 stand-alone  For OEL7.5 Install binary","File Folder with relative path":"database/MySQL/Doc/MySQL Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-install/mysql-8-0-13-stand-alone-for-oel-7-5-install-binary/","dgPassFrontmatter":true}
---


##  安装流程
### 确定平台版本和机器位数。
确定当前MYSQL版本对应各种操作系统 平台是否支持。

[https://www.mysql.com/support/supportedplatforms/database.html](https://www.mysql.com/support/supportedplatforms/database.html
) 
### 下载对应版本
#### 下载二进制安装包
-	[官方网站] http://dev.mysql.com/downloads/mirrors.html.
-	进入后可以选择对应的版本

> [!warning]
> tar.gz tar.xz 为二进制安装。
> RPM 为rpm 安装包。
> deb 为 deband linux 安装包.
> PKG 为mac 文件安装包。

#### 下载yum 源网站包（社区版）
[yum.repo.完整镜像包](https://dev.mysql.com/downloads/repo/yum/)

### 验证下载后文件是否正确
- LINUX  AND WINDOWS MD5 验证
```bash
shell> md5sum mysql-standard-8.0.13-linux-i686.tar.gz
aaab65abbec64d5e907dcd41b8699945  mysql-standard-8.0.13-linux-i686.tar.gz
```

```bash
shell> md5.exe mysql-installer-community-8.0.13.msi
aaab65abbec64d5e907dcd41b8699945  mysql-installer-community-8.0.13.msi
```

- RPM 包验证
```bash
shell> rpm --checksig MySQL-server-8.0.13-0.linux_glibc2.5.i386.rpm
MySQL-server-8.0.13-0.linux_glibc2.5.i386.rpm: md5 gpg OK
```
##  二进制部署
> [!warning]
> 使用二进制安装这种模式不同于yum 安装自动分析相关依赖包。必须首先移除mariadb或者以前的安装过mysql 的配置文件。例如/etc/my.conf.
### OS 环境准备
####  卸载mariadb
```bash
shell> yum remove mariadb*
```
#### 安装依赖包
```bash
shell> yum search libaio  # search for info
shell> yum install libaio # install library
```


#### 创建MySQL用户和组
```bash
shell> groupadd mysql
shell> useradd -r -g mysql -s /bin/false mysql
```
#### 解压软件

```bash
shell> cd /usr/local
shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz

shell> gunzip < /path/to/mysql-VERSION-OS.tar.gz | tar xvf -
```
#### 创建新的链接文件到真实目录
```bash
shell> ln -s full-path-to-mysql-VERSION-OS mysql
```
####  设置环境变量
```bash
shell> export PATH=$PATH:/usr/local/mysql/bin
```
#### 配置启动目录
```bash
shell> cd mysql
shell> mkdir mysql-files
shell> chown mysql:mysql mysql-files
shell> chmod 750 mysql-files
```

### 初始化数据库
#### 默认初始化方式
```bash
shell> bin/mysqld --initialize --user=mysql
```
#### 初始化数据库两种方式 
```bash
shell> bin/mysqld --initialize --user=mysql
shell> bin/mysqld --initialize-insecure --user=mysql
```


> [!warning]
> 两种初始化方式第一种必须强制使用密码登录。第二种方式可以使用跳过密码
> `
> shell> mysql -u root --skip-password
> `
> 然后更改对应密码
> `
> mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
> `
> 在mysql 8.0 默认加密的密码已经改了。现在默认使用caching_sha2_password 加密方式。
> 如果还需使用默认的mysql_native_password 加密方式的话使用默认 模式。
> 初始化的时候，默认路径为/usr/local/mysql

#### 使用命令行指定数据库集簇目录
- 1. 如果要改变安装目录使用以下目录：
```bash
shell> bin/mysqld --initialize --user=mysql
         --basedir=/opt/mysql/mysql
         --datadir=/opt/mysql/mysql/data
```
-  2. 配置文件
```bash
[mysqld]
basedir=/opt/mysql/mysql
datadir=/opt/mysql/mysql/data
```
#### 使用配置文件指定数据库集簇目录
1.  命令
```bash
shell> bin/mysqld --initialize-insecure --user=mysql \
--basedir=/app/mysql \
         --datadir=/app/mysql/mysql-files
```
2. 输出日志：
```bash
[root@mysql1 mysql]# bin/mysqld --initialize-insecure --user=mysql \
> --basedir=/app/mysql \
>          --datadir=/app/mysql/mysql-files
2018-09-28T16:18:32.178288Z 0 [System] [MY-013169] [Server] /app/mysql-commercial-8.0.12-el7-x86_64/bin/mysqld (mysqld 8.0.12-commercial) initializing of server in progress as process 5328
2018-09-28T16:18:46.268910Z 5 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2018-09-28T16:18:57.082553Z 0 [System] [MY-013170] [Server] /app/mysql-commercial-8.0.12-el7-x86_64/bin/mysqld (mysqld 8.0.12-commercial) initializing of server has completed
```

###  创建SSL 文件
####  说明

| **Format** | **Description** |
| --- | --- |
| [--datadir](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_datadir) | Path to data directory |
| [--help](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_help) | Display help message and exit |
| [--suffix](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_suffix) | Suffix for X509 certificate Common Name attribute |
| [--uid](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_uid) | Name of effective user to use for file permissions |
| [--verbose](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_verbose) | Verbose mode |
| [--version](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_version) | Display version information and exit |
####  SSL 密钥和证书的默认目录
```bash
shell> bin/mysql_ssl_rsa_setup
```
#### SSL 密钥和证书的指定目录
```bash
bin/mysql_ssl_rsa_setup --datadir=/app/mysql/mysql-files/
```

---

## YUM 源安装
### 安装步骤
1. 去下载MySQL 的repo 文件：[http://dev.mysql.com/downloads/repo/yum/](http://dev.mysql.com/downloads/repo/yum/) 
```bash
wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
```
2. 选择对应操作系统版本的。
3. 本地安装对应repo。

```bash
shell> sudo yum localinstall platform-and-version-specific-package-name.rpm
```

* EL6-based system 
```bash
shell> sudo yum localinstall mysql80013-community-release-el6-{version-number}.noarch.rpm
```
*  EL7-based system:
```bash
shell> sudo yum localinstall mysql80013-community-release-el7-{version-number}.noarch.rpm
```

* Fedora 28:
```bash
shell> sudo dnf localinstall mysql80013-community-release-fc28-{version-number}.noarch.rpm
```
* Fedora 27:
```bash
shell> sudo dnf localinstall mysql80013-community-release-fc27-{version-number}.noarch.rpm
```


### 示例：
```bash
yum localinstall mysql80-community-release-el7-1.noarch.rpm
```

* 查看已经启用MySQL 源
```bash
yum repolist enabled | grep "mysql.*-community.*"
```
* 启用集群安装
```bash
mysql-cluster
```
* 查看所有mysql 有关的安装源
```bash
shell> yum repolist all | grep mysql
```

* 开启对应安装yum源
```bash
shell> sudo yum-config-manager --disable mysql57-community
shell> sudo yum-config-manager --enable mysql80-community
```
* 例如：
开启集群
```bash
shell> sudo yum-config-manager --enable mysql-cluster-7.6-community
```

* 安装MYSQL
```bash
yum install mysql-community-server
```


##  使用RPM 包安装


> [!warning]
> 由于企业版只提供二进制和RPM 包方式。。不提供YUM 安装方式。
> 以下是各个RPM 包的含义：

| **Package Name** | **Summary** |
| --- | --- |
| mysql-commercial-**backup** | MySQL Enterprise Backup (added in 8.0.11) |
| mysql-commercial-**client** | MySQL client applications and tools |
| mysql-commercial-**common** | Common files for server and client libraries |
| mysql-commercial-**devel** | Development header files and libraries for MySQL database client applications |
| mysql-commercial-**embedded-compat** | MySQL server as an embedded library with compatibility for applications using version 18 of the library |
| mysql-commercial-**libs** | Shared libraries for MySQL database client applications |
| mysql-commercial-**libs-compat** | Shared compatibility libraries for previous MySQL installations; the version of the libraries matches the version of the libraries installed by default by the distribution you are using |
| mysql-commercial-**minimal-debuginfo** | Debug information for package mysql-commercial-server-minimal; useful when developing applications that use this package or when debugging this package |
| mysql-commercial-**server** | Database server and related tools |
| mysql-commercial-**server-minimal** | Minimal installation of the database server and related tools (added in 8.0.0) |
| mysql-commercial-**test** | Test suite for the MySQL server |
1. 下载对应版本的RPM 包进行解压
```bash
[root@mysql3 ~]# unzip V979091-01.zip 
Archive:  V979091-01.zip
 extracting: mysql-commercial-libs-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-embedded-compat-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-test-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-server-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-server-minimal-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-minimal-debuginfo-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-devel-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-client-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-libs-compat-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-common-8.0.12-1.1.el7.x86_64.rpm  
 extracting: mysql-commercial-backup-8.0.12-1.1.el7.x86_64.rpm  
 extracting: README.txt
```
2. 批量自动安装：自动解决依赖包。使用此命令时注意不要和server_mini 混用。
```bash
yum install mysql-commercial-*
```

* 简易服务端
```bash
sudo yum install mysql-community-{server,client,common,libs}-* 
```
* 简易客户端
```bash
yum install mysql-community-{client,common,libs}-* 
```
* 目录分布

| **Files or Resources** | **Location** |
| --- | --- |
| Client programs and scripts | /usr/bin |
| **[mysqld](file:///D:/refman-8.0-en.html-chapter/programs.html#mysqld "4.3.1 mysqld — The MySQL Server")** server | /usr/sbin |
| Configuration file | /etc/my.cnf |
| Data directory | /var/lib/mysql |
| Error log file | For RHEL, Oracle Linux, CentOS or Fedora platforms: /var/log/mysqld.logFor SLES: /var/log/mysql/mysqld.log |
| Value of **[secure\_file\_priv](file:///D:/refman-8.0-en.html-chapter/server-administration.html#sysvar_secure_file_priv)** | /var/lib/mysql-files |
| System V init script | For RHEL, Oracle Linux, CentOS or Fedora platforms: /etc/init.d/mysqldFor SLES: /etc/init.d/mysql |
| Systemd service | For RHEL, Oracle Linux, CentOS or Fedora platforms: mysqldFor SLES: mysql |
| Pid file | /var/run/mysql/mysqld.pid |
| Socket | /var/lib/mysql/mysql.sock |
| Keyring directory | /var/lib/mysql-keyring |
| Unix manual pages | /usr/share/man |
| Include (header) files | /usr/include/mysql |
| Libraries | /usr/lib/mysql |
| Miscellaneous support files (for example, error messages, and character set files) | /usr/share/mysql |
###  安装相关组件
列出开启yum 中所有相关的安装包。，包括未enable 中的源
例如主机上要连接低版本客户端。则需要安装低版本客户端如下：
```bash
rpm --oldpackage -ivh mysql-community-libs-5.5.50-2.el6.x86_64.rpm
```
> 安装其他软件：到官网下载对应版本安装。
## 启动和关闭
### 使用二进制安装
```bash
 shell> bin/mysqld_safe --user=mysql &
# 6. Next command is optional
```
#### 拷贝自启动文件
```bash
shell> cp /app/mysql/support-files/mysql.server /etc/init.d/
```
#### 编辑my.cnf 文件
```bash
basedir=/app/mysql
datadir=/app/mysql/mysql-files
```
####  重载启动项目
```bash
systemctl  daemon-reload
```
#### 查看是否启动MySQL
```bash
systemctl  status mysql
```
#### 开机自启动

> [!warning]
> > 由于使用二进制安装,则需要配置chkconfig 。RPM 包不需要配置此选项。


```bash
chkconfig --add mysql.server
chkconfig mysql.server on
```
### 使用yum 源安装
#### 启动MySQL
```bash
service mysqld start
systemctl  start mysqld
```
#### 查看是否启动MySQL
```bash
systemctl  status mysqld
```
#### 开机自启动
```bash
systemctl  enable mysqld
systemctl disable mysqld
```
###  使用RPM 包安装
#### 启动MySQL
```bash
service mysqld start
systemctl  start mysqld
```
#### 查看是否启动MySQL
```bash
systemctl  status mysqld
```
####  开机自启动
```bash
systemctl  enable mysqld
systemctl disable mysqld
```
## 账户安全配置
### yum 源安装
* 临时密码：
```sql
grep 'temporary password' /var/log/mysqld.log
```
* 登录：
```bash
mysql -uroot -p 
```
* 更改密码：
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

* 由于密码复杂度有相关要求：但是测试不需要则

```sql
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)

set GLOBAL validate_password.policy=0;
设置密码长度
set GLOBAL validate_password.length=3;
```
* 设置密码复杂度为低。更方便测试。

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```

### 二进制安装
* 使用mysqld --insecure初始化后的密码更改

```sql
mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

* 如果使用mysqld --initialize-insecure 进行初始化
则：
```bash
mysql -u root --skip-password
```

* 更改密码：
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '123'
```
### RPM 包安装
* 临时密码：
```bash
grep 'temporary password' /var/log/mysqld.log
```
* 登录：
```bash
mysql -uroot -p 
```
* 更改密码：
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

* 由于密码复杂度有相关要求：但是测试不需要则

```sql
mysql> SHOW VARIABLES LIKE 'validate_password%';

+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)

mysql> set GLOBAL validate_password.policy=0;
mysql> set GLOBAL validate_password.length=3;
```

* 设置密码复杂度为低。更方便测试。

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```
## 附录

### MySQL 启动后诊断
#### 查看日志
```bash
tail host_name.err
```
####  选择驱动
默认为InnoDB
#### 确认数据文件位置是否合适
#### 查看所有配置参数和所有的环境变量
```bash
mysqld --basedir=/app/mysql --verbose –help | more
```
####  配置文件环境变量
```bash
mysqladmin variables -u root -p 
mysqladmin -h host_name variables - u root -p 
```

### 测试MySQL
* 检查mysql 是否正常运行
```bash
mysqladmin version
```

* 检查配置变量值
```bash
mysqladmin variables
```
* 检查是否能登陆
```bash
mysqladmin -u root -p version
```
* 关闭mysql 服务
```bash
mysqladmin -u root shutdown
```

* 显示mysql 数据库
```bash
mysqlshow
```
* 显示某个数据库中的表
```bash
mysqlshow mysql
```
* shell 界面查询表数据
```bash
mysql -e "SELECT User, Host, plugin FROM mysql.user" mysql
```

> [!note]
> 以上命令成功执行后则mysql 数据正常。

### 忘记root 密码
* 方法1：
```bash
kill `cat /mysql-data-directory/host_name.pid`
```

* 把以下语句保存到文件中
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```
例如：/root/mysql-init
* 开始改变密码
```bash
mysqld --init-file=/home/me/mysql-init &
```
mysql 服务会自动启动。

* 然后删除保存密码的文件。
方法2：
添加my.cnf 选项
```
skip-grant-tables:
```
* 重启MySQL
* 更改密码
```sql
mysql
update user set password=password("new_pass") where user="root";
FLUSH PRIVILEGES;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

