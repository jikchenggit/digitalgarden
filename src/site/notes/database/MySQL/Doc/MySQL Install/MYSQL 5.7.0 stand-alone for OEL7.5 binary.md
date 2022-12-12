---
{"author":"aming","email":"jikcheng@163.com","title":"MYSQL 5.7.0 stand-alone for OEL7.5 binary","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:07","tags":"MYSQL 5.7.0 stand-alone for OEL7.5 binary","File Folder with relative path":"database/MySQL/Doc/MySQL Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-install/mysql-5-7-0-stand-alone-for-oel-7-5-binary/","dgPassFrontmatter":true}
---



# 2. 安装流程
## 2.1. 确定平台版本和机器位数。
确定当前MYSQL版本对应各种操作系统 平台是否支持。

[https://www.mysql.com/support/supportedplatforms/database.html](https://www.mysql.com/support/supportedplatforms/database.html
) 
## 2.2. 下载对应版本
### 2.2.1. 下载二进制安装包
	官方网站
	http://dev.mysql.com/downloads/mirrors.html.
	进入后可以选择对应的版本
> NOTE:
tar.gz tar.xz 为二进制安装。
RPM 为rpm 安装包。
deb 为 deband linux 安装包.
PKG 为mac 文件安装包。

### 2.2.2. 下载yum 源配置包（社区版）
[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)
### 2.2.3. 下载源码编译
	源码现目前在GIT 上托管。
## 2.3. 验证下载后文件是否正确
* 	LINUX  AND WINDOWS MD5 验证
```sh
shell> md5sum mysql-standard-8.0.13-linux-i686.tar.gz
aaab65abbec64d5e907dcd41b8699945  mysql-standard-8.0.13-linux-i686.tar.gz
```

```sh
shell> md5.exe mysql-installer-community-8.0.13.msi
aaab65abbec64d5e907dcd41b8699945  mysql-installer-community-8.0.13.msi
```

* RPM 包验证
```sh
shell> rpm --checksig MySQL-server-8.0.13-0.linux_glibc2.5.i386.rpm
MySQL-server-8.0.13-0.linux_glibc2.5.i386.rpm: md5 gpg OK
```
# 3. 二进制部署
> NOTE:
使用二进制安装这种模式不同于yum 安装自动分析相关依赖包。必须首先移除mariadb或者以前的安装过mysql 的配置文件。例如/etc/my.conf.

* 卸载mariadb
```sh
shell> yum remove mariadb*
```
* 安装依赖包
```sh
shell> yum search libaio  # search for info
shell> yum install libaio # install library
```
## 3.1. 安装目录说明
## 3.2. 安装MySQL
### 3.2.1. 创建MySQL用户和组
```sh
shell> groupadd mysql
shell> useradd -r -g mysql -s /bin/false mysql
```
### 3.2.2. 解压软件

```sh
shell> cd /usr/local
shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz

shell> gunzip < /path/to/mysql-VERSION-OS.tar.gz | tar xvf -
```
### 3.2.3. 创建新的链接文件到真实目录
```sh
shell> ln -s full-path-to-mysql-VERSION-OS mysql
```
### 3.2.4. 设置环境变量
```sh
shell> export PATH=$PATH:/usr/local/mysql/bin
```
### 3.2.5. 配置启动目录
```sh
shell> cd mysql
shell> mkdir mysql-files
shell> chown mysql:mysql mysql-files
shell> chmod 750 mysql-files
```
### 3.2.6. 初始化数据库
```sh
shell> bin/mysqld --initialize --user=mysql
```
#### 3.2.6.1. 初始化数据库两种方式 
```sh
shell> bin/mysqld --initialize --user=mysql
shell> bin/mysqld --initialize-insecure --user=mysql
```

NOTE:
>两种初始化方式第一种必须强制使用密码登录
第二种方式可以使用跳过密码
`
shell> mysql -u root --skip-password
`
然后更改对应密码
`
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
`
在mysql 8.0 默认加密的密码已经改了。现在默认使用caching_sha2_password 加密方式。
如果还需使用默认的mysql_native_password 加密方式的话使用默认 模式。
初始化的时候，默认路径为/usr/local/mysql

#### 3.2.6.2. 更改默认路径
* 如果要改变安装目录使用以下目录：
```sh
shell> bin/mysqld --initialize --user=mysql
         --basedir=/opt/mysql/mysql
         --datadir=/opt/mysql/mysql/data
```
* 配置文件
```sh
[mysqld]
basedir=/opt/mysql/mysql
datadir=/opt/mysql/mysql/data
```
* 使用配置文件指定位置:
* 范例
```sh
shell> bin/mysqld --initialize-insecure --user=mysql \
--basedir=/app/mysql \
         --datadir=/app/mysql/mysql-files
```
* 输出日志：
```sh
[root@mysql1 mysql]# bin/mysqld --initialize-insecure --user=mysql \
> --basedir=/app/mysql \
>          --datadir=/app/mysql/mysql-files
2018-09-28T16:18:32.178288Z 0 [System] [MY-013169] [Server] /app/mysql-commercial-8.0.12-el7-x86_64/bin/mysqld (mysqld 8.0.12-commercial) initializing of server in progress as process 5328
2018-09-28T16:18:46.268910Z 5 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2018-09-28T16:18:57.082553Z 0 [System] [MY-013170] [Server] /app/mysql-commercial-8.0.12-el7-x86_64/bin/mysqld (mysqld 8.0.12-commercial) initializing of server has completed
```

### 3.2.7. 创建安全连接
#### 3.2.7.1. 说明

| **Format** | **Description** |
| --- | --- |
| [--datadir](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_datadir) | Path to data directory |
| [--help](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_help) | Display help message and exit |
| [--suffix](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_suffix) | Suffix for X509 certificate Common Name attribute |
| [--uid](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_uid) | Name of effective user to use for file permissions |
| [--verbose](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_verbose) | Verbose mode |
| [--version](file:///D:/refman-8.0-en.html-chapter/programs.html#option_mysql_ssl_rsa_setup_version) | Display version information and exit |
#### 3.2.7.2. 默认目录
```sh
shell> bin/mysql_ssl_rsa_setup
```
#### 3.2.7.3. 指定目录
```sh
bin/mysql_ssl_rsa_setup --datadir=/app/mysql/mysql-files/
```
----------------------到此安装完毕-----------------------------------
# 4. YUM 源安装
a.	去下载MySQL 的repo 文件：[http://dev.mysql.com/downloads/repo/yum/](http://dev.mysql.com/downloads/repo/yum/) 
```sh
wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
```
b.	选择对应操作系统版本的
c.	本地安装

```sh
shell> sudo yum localinstall platform-and-version-specific-package-name.rpm
```

* EL6-based system 
```sh
shell> sudo yum localinstall mysql80013-community-release-el6-{version-number}.noarch.rpm
```
*  EL7-based system:
```sh
shell> sudo yum localinstall mysql80013-community-release-el7-{version-number}.noarch.rpm
```

* Fedora 28:
```sh
shell> sudo dnf localinstall mysql80013-community-release-fc28-{version-number}.noarch.rpm
```
* Fedora 27:
```sh
shell> sudo dnf localinstall mysql80013-community-release-fc27-{version-number}.noarch.rpm
```


* 范例：
```sh
yum localinstall mysql80-community-release-el7-1.noarch.rpm
```

* 查看已经启用MySQL 源
```sh
yum repolist enabled | grep "mysql.*-community.*"
```
* 启用集群安装
```sh
mysql-cluster
```
* 查看所有mysql 有关的安装源
```sh
shell> yum repolist all | grep mysql
```

* 开启对应安装yum源
```sh
shell> sudo yum-config-manager --disable mysql57-community
shell> sudo yum-config-manager --enable mysql80-community
```
* 例如：
开启集群
```sh
shell> sudo yum-config-manager --enable mysql-cluster-7.6-community
```

* 安装MYSQL
```sh
yum install mysql-community-server
```


# 5. 使用RPM 包安装

说明：
> 由于企业版只提供二进制和RPM 包方式。。不提供YUM 安装方式。
以下是各个RPM 包的含义：

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
```sh
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
```sh
yum install mysql-commercial-*
```

* 简易服务端
```sh
sudo yum install mysql-community-{server,client,common,libs}-* 
```
* 简易客户端
```sh
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
## 5.1. 安装相关组件
列出开启yum 中所有相关的安装包。，包括未enable 中的源
例如主机上要连接低版本客户端。则需要安装低版本客户端如下：
```sh
rpm --oldpackage -ivh mysql-community-libs-5.5.50-2.el6.x86_64.rpm
```
> 安装其他软件：到官网下载对应版本安装。
# 6. 启动和关闭
## 6.1. 使用二进制安装
```sh
 shell> bin/mysqld_safe --user=mysql &
# 6. Next command is optional
```
### 6.1.1. 拷贝自启动文件
```sh
shell> cp /app/mysql/support-files/mysql.server /etc/init.d/
```
### 6.1.2. 编辑my.cnf 文件
```sh
basedir=/app/mysql
datadir=/app/mysql/mysql-files
```
### 6.1.3. 重载启动项目
```sh
systemctl  daemon-reload
```
### 6.1.4. 查看是否启动MySQL
```sh
systemctl  status mysql
```
### 6.1.5. 开机自启动
NOTE:
> 由于使用二进制安装,则需要配置chkconfig 。RPM 包不需要
```sh
chkconfig --add mysql.server
chkconfig mysql.server on
```
## 6.2. 使用yum 源安装
### 6.2.1. 启动MySQL
```sh
service mysqld start
systemctl  start mysqld
```
### 6.2.2. 查看是否启动MySQL
```sh
systemctl  status mysqld
```
### 6.2.3. 开机自启动
```sh
systemctl  enable mysqld
systemctl disable mysqld
```
## 6.3. 使用RPM 包安装
### 6.3.1. 启动MySQL
```sh
service mysqld start
systemctl  start mysqld
```
### 6.3.2. 查看是否启动MySQL
```sh
systemctl  status mysqld
```
### 6.3.3. 开机自启动
```sh
systemctl  enable mysqld
systemctl disable mysqld
```
# 7. MySQL 启动后诊断
## 7.1. 查看日志
```sh
tail host_name.err
```
## 7.2. 选择驱动
默认为InnoDB
## 7.3. 确认数据文件位置是否合适
## 7.4. 查看所有配置参数和所有的环境变量
```sh
mysqld --basedir=/app/mysql --verbose --help | more
```
## 7.5. 配置文件环境变量
```sh
mysqladmin variables
mysqladmin -h host_name variables
```
# 8. 测试MySQL
* 检查mysql 是否正常运行
```sh
mysqladmin version
```

* 检查配置变量值
```sh
mysqladmin variables
```
* 检查是否能登陆
```sh
mysqladmin -u root -p version
```
* 关闭mysql 服务
```sh
mysqladmin -u root shutdown
```

* 显示mysql 数据库
```sh
mysqlshow -u -p
```
* 显示某个数据库中的表
```sh
mysqlshow mysql -u -p
```
* shell 界面查询表数据
```sh
mysql -e "SELECT User, Host, plugin FROM mysql.user" mysql -u root -p
```

NOTE:
> 以上命令成功执行后则mysql 数据正常。

# 9. 账户安全
## 9.1. yum 源安装
* 查找密码：
```sql
grep 'temporary password' /var/log/mysqld.log
```
* 登录：
```sh
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

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```

## 9.2. 二进制安装
* 使用mysqld --insecure初始化后的密码更改

```sql
mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

* 如果使用mysqld --initialize-insecure 进行初始化
则：
```sh
mysql -u root --skip-password
```

* 更改密码：
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234'
```
## 9.3. RPM 包安装
* 查找密码：
```sh
grep 'temporary password' /var/log/mysqld.log
```
* 登录：
```sh
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

```sh
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```
# 10. 附录
## 10.1. 忘记root 密码
* 方法1：
```sh
kill `cat /mysql-data-directory/host_name.pid`
```

* 把以下语句保存到文件中
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```
例如：/root/mysql-init
* 开始改变密码
```sh
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
FLUSH PRIVILEGES;
update user set password=password("new_pass") where user="root";
FLUSH PRIVILEGES;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

