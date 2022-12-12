---
{"author":"aming","email":"jikcheng@163.com","title":"mongodb Install","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"mongodb Install","File Folder with relative path":"database/mongodb/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/mongodb/doc/mongodb-install/","dgPassFrontmatter":true}
---



## 安装repo
```bash
cat > /etc/yum.repos.d/mongodb-org-5.0.repo <<   
[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc
EOF
```
## 安装软件包
手动下载地址:
[https://repo.mongodb.org/yum/redhat/](https://repo.mongodb.org/yum/redhat/)

```bash
sudo yum install -y mongodb-org
```

### 安装指定版本
```bash
sudo yum install -y mongodb-org-5.0.1 mongodb-org-database-5.0.1 mongodb-org-server-5.0.1 mongodb-org-shell-5.0.1 mongodb-org-mongos-5.0.1 mongodb-org-tools-5.0.1
```
## 操作系统配置

Linux  7 
```bash
4096
/etc/security/limits.d/20-nproc.conf 
```
Linux  6 
```bash
1024
/etc/security/limits.d/90-nproc.conf
```
Linux 8 

-f (file size): unlimited
-t (cpu time): unlimited
-v (virtual memory): unlimited [1]
-l (locked-in-memory size): unlimited
-n (open files): 64000
-m (memory size): unlimited [1] [2]
-u (processes/threads): 64000

/etc/security/limits.conf 

```bash
mongod    soft     nofile 64000
mongod    hard     nofile 64000
mongod    soft     nproc 64000
mongod    hard     nproc 64000
```

## mongodb 默认位置
```bash
/var/lib/mongo
/var/log/mongodb
```
## 指定mongodb 数据位置
1. 创建目录.
2. 编辑`/etc/mongod.conf` 文件
```bash
storage.dbPath 
systemLog.path
```
3. 更改权限
```bash
sudo chown -R mongod:mongod /u01/mongodb;
```
## 配置SELIUNX
1. 安装配置工具
```bash
sudo yum install checkpolicy
```
2. 创建配置策略文件

```bash
cat > mongodb_cgroup_memory.te <<EOF
module mongodb_cgroup_memory 1.0;
require {
      type cgroup_t;
      type mongod_t;
      class dir search;
      class file { getattr open read };
}
#============= mongod_t ==============
allow mongod_t cgroup_t:dir search;
allow mongod_t cgroup_t:file { getattr open read };
EOF

```
3. 加载策略
```bash
 checkmodule -M -m -o mongodb_cgroup_memory.mod mongodb_cgroup_memory.te
semodule_package -o mongodb_cgroup_memory.pp -m mongodb_cgroup_memory.mod
sudo semodule -i mongodb_cgroup_memory.pp
```
### 配置netstat 访问策略

```bash
cat > mongodb_proc_net.te <<EOF
module mongodb_proc_net 1.0;
require {
    type proc_net_t;
    type mongod_t;
    class file { open read };
}
#============= mongod_t ==============
allow mongod_t proc_net_t:file { open read };
EOF
```
### 加载策略

```bash
checkmodule -M -m -o mongodb_proc_net.mod mongodb_proc_net.te
semodule_package -o mongodb_proc_net.pp -m mongodb_proc_net.mod
sudo semodule -i mongodb_proc_net.pp
```

## 启动mongodb
sudo systemctl start mongod
sudo systemctl daemon-reload

sudo systemctl status mongod
sudo systemctl enable mongod
sudo systemctl stop mongod
sudo systemctl restart mongod