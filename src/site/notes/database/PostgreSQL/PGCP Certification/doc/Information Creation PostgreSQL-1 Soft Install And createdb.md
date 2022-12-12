---
{"author":"aming","email":"jikcheng@163.com","title":"Information Creation PostgreSQL-1 Soft Install And createdb","creation_date":"2022-11-03 08:45","Last modified date":"2022-11-27 19:15","tags":"Information Creation PostgreSQL-1 Soft Install And createdb","File Folder with relative path":"database/PostgreSQL/PGCP Certification/doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/postgre-sql/pgcp-certification/doc/information-creation-postgre-sql-1-soft-install-and-createdb/","dgPassFrontmatter":true}
---



## 操作系统配置
### 开发防火墙端口
```bash
systemctl status firewalld
firewall-cmd --permanent --zone=public --add-port=1922/tcp
firewall-cmd --reload
```
### 创建用户
```bash
root>
groupadd postgres
useradd -g postgres postgres
```
### 创建相关目录
```bash
root>
mkdir -p  /soft
mkdir -p /usr/local/pg12.2
chown postgres:postgres /soft  /usr/local/pg12.2
```
###  配置用户环境变量
```bash
postgres> 
cat >> ~/.bash_profile << EOF
export PGPORT=1922
export PG_HOME=/usr/local/pg12.2
export PATH=\$PG_HOME/bin:\$PATH
export PGDATA=\$PG_HOME/data
export LD_LIBRARY_PATH=\$PG_HOME/lib
export LANG=en_US.utf8
EOF
source ~/.bash_profile
```

### 配置内核参数
```bash
root>
cat >> /etc/sysctl.conf << EOF
kernel.shmmax= 68719476736（默认）#最大共享内存段大小
kernel.shmall= 4294967296（默认）#可以使用的共享内存的总量
kernel.shmmni= 4096#整个系统共享内存段的最大数目
kernel.sem= 50100 64128000 50100 1280 #每个信号对象集的最大信号对象数
fs.file-max = 7672460 #文件句柄的最大数量。
net.ipv4.ip_local_port_range= 9000 65000 #应用程序可使用的IPv4端口范围
net.core.rmem_default= 1048576 #套接字接收缓冲区大小的缺省值
net.core.wmem_default= 262144 #套接字发送缓冲区大小的缺省值
net.core.wmem_max= 1048576 #套接字发送缓冲区大小的最大值
EOF
# 生效内核参数
sysctl -p
```

## 源代码安装
### 下载源码包
```bash
postgres> 
cd /soft
wget https://ftp.postgresql.org/pub/source/v12.12/postgresql-12.12.tar.gz
```


### 安装软件依赖包
#### 最小依赖包
```bash
root>
yum install gcc gcc-c++ zlib-devel readline-devel wget psmisc -y 
```
#### 全部依赖包
```bash
root>
yum install gcc gcc-c++ zlib-devel readline-devel wget psmisc perl-ExtUtils-Embed pam-devel libxml2-devel libxslt-devel openldap-devel python39-devel python2-devel openssl-devel cmake wget -y 
```

```ad-warning
这里用python2 还是python3  ，官网两种都支持。所以都安装。
```
快照为：DB_INSTALL_ENV

---

### 安装步骤
#### 编译postgresql 软件包并安装到指定目录
```bash
postgres>
cd /soft/
tar -zxf postgresql-12.2.tar.gz
cd /soft/postgresql-12.2
./configure --prefix=/usr/local/pg12.2
make -j 20
make install
```

##### **Configure常用配置选项：**
| 选项             | 说明                                  |
| ---------------- | ------------------------------------- |
| prefix           | 指定软件的安装路径                    |
| with-openssl     | 对openssl进行扩展支持                 |
| with-python      | 对python进行扩展支持                  |
| with-perl        | 对perl进行扩展支持                    |
| with-libxml      | 对xml进行扩展支持                     |
| --with-pgport    | 数据库端口号                          |
| --with-tcl       | 对C 语言扩展支持                      |
| --with-pam       | 使用PAM(可插入身份验证模块)支持构建。 |
| --with-libxml    | 对 libxml2 扩展                       |
| --with-blocksize | 数据库块大小。                        | 

```bash
./configure --prefix=/usr/local/pg12.2 --with-pgport=1922 --with-openssl --with-perl --with-tcl --with-python  --without-ldap --with-libxml --with-libxslt --enable-thread-safety --with-wal-blocksize=16 --with-blocksize=8 --enable-dtrace --enable-debug
```


##### --with-blocksize**
•如果数据库需要经常做插入的操作，数据量增长非常快，尽量把此参数设大一点；
•经常做小数据查询、更新且内存不是非常大的时候可以设小一点，默认8K即可。
•生产环境不要加--enable-dtrace --enable-debug。


#### 编译第三方插件并安装
```bash
#包括第三方插件全部编译
gmake world -j 20
#这个需要使用普通用户执行，可选，耗时较长
gmake check-world  -j 20
#包括第三方插件全部安装
gmake install
```

```ad-note
gmake world包含了所有文档和所有contirb 。

```
#### 创建数据库集簇
##### 创建目录
```bash
postgres>
mkdir -p /usr/local/pg12.2/data
```
##### 初始化数据库集簇
```bash
postgres>
initdb -D $PGDATA -W --data-checksums  
```

```ad-note
复制时需要 data-cecksums 支持块校验。
```
##### 启动数据库集簇
```bash
postgres>
pg_ctl -D $PGDATA -l logfile start
```
##### 创建新的数据库
```bash
postgres>
createdb  testdb
```
##### 登录数据库
```bash
psql testdb
```

#### 配置数据库网络和认证

##### 配置监听
```bash
postgres>
cat >> $PG_HOME/data/postgresql.conf << EOF
listen_addresses = '*' 
EOF
```
##### 客户端认证文件
```bash
postgres>
cat >> $PG_HOME/data/pg_hba.conf << EOF
host    all             all             192.168.10.0/24            trust
EOF
```


##### 配置ERROR LOG 
```bash
psqlgres> cat >> $PGDATA/err_log.conf << EOF
log_destination = 'csvlog'
logging_collector = on
log_directory = 'pg_log'
log_filename = 'postgresql-%Y-%m-%d'
log_truncate_on_rotation = off
log_rotation_age = 1d
log_rotation_size = 0
log_error_verbosity = verbose
log_statement = all
EOF

cat >> $PGDATA/postgresql.conf << EOF
include_if_exists='err_log.conf'
EOF
```
##### 重启数据库
```bash
postgres>
pg_ctl restart -D $PGDATA
```

##### 测试远程登陆
```bash
psql -h 192.168.10.230 -U postgres -d postgres
```
## 导入测试数据
[[database/PostgreSQL/PGCP Certification/doc/PGCP （M）环境准备\|PGCP （M）环境准备]]


