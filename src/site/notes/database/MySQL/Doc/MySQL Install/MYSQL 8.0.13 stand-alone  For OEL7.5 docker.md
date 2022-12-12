---
{"author":"aming","email":"jikcheng@163.com","title":"MYSQL 8.0.13 stand-alone  For OEL7.5 docker","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:07","tags":"MYSQL 8.0.13 stand-alone  For OEL7.5 docker","File Folder with relative path":"database/MySQL/Doc/MySQL Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-install/mysql-8-0-13-stand-alone-for-oel-7-5-docker/","dgPassFrontmatter":true}
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
# 3. 使用docer安装部署MYSQL到linux
## 3.1. 安装docker
### 3.1.1. 删除旧版本docker
```sh
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```
### 3.1.2. 安装依赖包
```sh
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
### 3.1.3. 安装docker repo yum 源
```sh
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
### 3.1.4. 开启yum 源安装
```sh
yum-config-manager --enable docker-ce-edge
```
* 开启套件
```sh
yum-config-manager --enable docker-ce-test
```
### 3.1.5. 安装对应的contanier linux
```sh
yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.68-1.el7.noarch.rpm
```
### 3.1.6. 安装docker
```sh
yum install docker-ce
```
### 3.1.7. 查看是否安装成功
```sh
yum list docker-ce --showduplicates | sort -r
```
### 3.1.8. 安装DOCKER 的其他版本
```sh
yum install docker-ce-<VERSION STRING>
```
### 3.1.9. 启动docker
```sh
systemctl start docker
```
### 3.1.10. 自启动
```sh
 systemctl enable docker
```
### 3.1.11. 检测是否正常
```sh
docker run hello-world
```
## 3.2. 安装MySQL
### 3.2.1. 下载对应版本
```sh
docker pull  store/oracle/mysql-server
```
* 开源版
```sh
docker pull  mysql/mysql-server
```
* 企业版
```sh
docker pull  store/oracle/mysql-enterprise-server:8.0
```
### 3.2.2. 查看镜像
```sh
docker images
```
# 4. 启动和关闭
## 4.1. 从镜像中启动MySQL
```sh
docker run --name=mysql1 -d mysql/mysql-server1
```
* 启动MySQL
```sh
docker start mysql1
```
* 停止MySQL
```sh
docker stop mysql1
```
* 重启MySQL
```sh
docker restart mysql1 
```
## 4.2. 查看是否启动MySQL
```sh
docker ps
```
# 5. 启动后诊断
## 5.1. 查看日志
```sh
docker logs mysql1
```
* 连接到docker 容器中
```sh
docker exec -it mysql1 bash 
```
## 5.2. 查看日志
```sh
tail host_name.err
```
## 5.3. 选择驱动
默认为InnoDB
## 5.4. 确认数据文件位置是否合适
## 5.5. 查看所有配置参数和所有的环境变量
```sh
mysqld --basedir=/app/mysql --verbose –help | more
```
## 5.6. 配置文件环境变量
```sh
mysqladmin variables
mysqladmin -h host_name variables
```
* 登录：
```sh
mysql -uroot -p 
```
* 更改密码：
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```



# 6. 测试MySQL
* 连接到docker 容器中
```sh
docker exec -it mysql1 bash 
```
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
mysqlshow
```
* 显示某个数据库中的表
```sh
mysqlshow mysql
```
* shell 界面查询表数据
```sh
mysql -e "SELECT User, Host, plugin FROM mysql.user" mysql
```

NOTE:
> 以上命令成功执行后则mysql 数据正常。
# 7. 账户安全
* 使用其他方式初始化后的密码更改
```sh
mysql -u root -p
docker logs mysql1 2>&1 | grep GENERATED
```
* docker更改密码：

```sh
docker exec -it mysql1 mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '3345091';
```
# 8. 升级
## 8.1. 停止要升级的容器
```sh
docker stop mysql57
```
## 8.2. 拉取mysql
* 社区版

```sh
docker pull mysql/mysql-server
```
* 企业版
```sh
docker pull store/oracle/mysql-enterprise-server
```
## 8.3. 启动新的容器升级
* 社区版
```sh
docker run --name=mysql80 \
   --mount type=bind,src=/path-on-host-machine/my.cnf,dst=/etc/my.cnf \
   --mount type=bind,src=/path-on-host-machine/datadir,dst=/var/lib/mysql \        
   -d mysql/mysql-server:8.0
```
* 企业版
```sh
docker run --name=mysql80 \
   --mount type=bind,src=/path-on-host-machine/my.cnf,dst=/etc/my.cnf \
   --mount type=bind,src=/path-on-host-machine/datadir,dst=/var/lib/mysql \        
   -d store/oracle/mysql-enterprise-server:8.0
```

## 8.4. 启动升级
```sh
docker exec -it mysql80 mysql_upgrade -uroot -p
```
## 8.5. 完成升级后重启
```sh
docker restart mysql80
```


# 9. 附录
## 9.1. 忘记root 密码
* docker 下的mysql 使用方式
启动容器时指定选项
```sh
docker run --name mysql1 -d mysql/mysql-server --character-set-server=utf8mb4 --collation-server=utf8mb4_col
```

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
--skip-grant-tables:
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