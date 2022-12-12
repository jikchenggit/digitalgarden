---
{"author":"aming","email":"jikcheng@163.com","title":"kingbase Clusterware Install","creation_date":"2022-10-28 18:14","Last modified date":"2022-11-27 18:58","tags":"kingbase Clusterware Install","File Folder with relative path":"database/Kingbase/Doc/Cluster manager/RAC/misc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/kingbase/doc/cluster-manager/rac/doc/kingbase-clusterware-install/","dgPassFrontmatter":true}
---



## 本章背景知识
目前支持两种方式搭建集群：
1、使用`cluster_manager.sh` 搭建集群。
2、手工搭建集群
## 使用cluster_manager.sh 搭建集群

### 命令帮助

```
sh cluster_manager.sh --help  
Usage: cluster_manager.sh <options>  
<options> may be:  
--help show this help, then exit  
-A, --all config all(default) incldue init_db  
--exclude_data_init config all(default) but exclude  
init_db qdisk_init config_resouce  
--config_host config host  
--clean_host clean host  
--qdisk_init init votingdisk  
--config_corosync config corosync  
--start_corosync start corosync  
--stop_corosync stop corosync  
--status_corosync corosync status  
--add_pacemaker_user add pacemaker daemon user  
--remove_pacemaker_user remove pacemaker daemon user  
--add_env add env variable  
--clean_env clean env variable  

--config_pacemaker config pacemaker  
--start_pacemaker start pacemaker  
--stop_pacemaker stop pacemaker  
--status_pacemaker pacemaker status  
--config_qdevice config qdevice  
--start_qdevice start qdevice  
--stop_qdevice stop qdevice  
--status_qdevice qdevice status  
--config_datadir config filesystem dir  
--config_kingbase config kingbase log dir  
--init_db init kingbase database  
--config_resource crm configure resource  
--stop_resource stop resource  
--mount_dir mount dir  
--umount_dir umount dir  
--clean_all clean all config but not for  
kingbase data  
--start start corosync, wait for quorate, start pacemaker and qdevice  
--start_nowait start corosync, pacemaker and qdevice, do not wait for quorate  
--stop stop qdevice pacemaker corosync  
--status show corosync pacemaker qdevice status
```

### node1

#### 配置cluster_manager.sh 脚本
```bash
install_dir=/opt/KingbaseHA
votingdisk=/dev/sdk
cluster_name=kcluster
use_esxi=0
use_ipmi=1
ipmi_server=(192.168.10.224 192.168.10.225)
ipmi_user=(root root)
ipmi_passwd=("kingbase.123" "kingbase.123")
##FIP
fip=(192.168.10.223 192.168.10.224)
fipinterface=ens224
netmask=24
##pacemaker
pacemaker_daemon_group=haclient
pacemaker_daemon_user=hacluster
env_bash_file=/root/.bashrc
##kingbase array should match
data_disk=("-U ab5b1a90-8762-48d3-a00a-46729d208c74" "-U 9c7d9e17-ebc5-435c-84c9-44b32ae87d9e")
data_dir=(/sharedata/data1/ /sharedata/data2/)
node_name=(node1 node2)
node_ip=(192.168.10.224 192.168.10.225)
port=(36321 36322)
fstype=xfs
kingbaseowner=kingbase
kingbasegroup=kingbase
kingbase_install_dir=/KingbaseES/V8/Server
##pingd
gateway=192.168.10.1

##crm_dsn.conf
database="test"
username="system"
# If loged in to database without password,
# the item of password could not be provided.
password="kingbase"

ping_gateway="192.168.10.1"
```
全部配置请参见 ![[cluster_manager.sh]]
```ad-warning
磁盘每次格式化后，都需要重新取UUID。
[root@node1 ~]#  blkid /dev/sdc
/dev/sdc: UUID="9c7d9e17-ebc5-435c-84c9-44b32ae87d9e" BLOCK_SIZE="512" TYPE="xfs"
[root@node1 ~]# blkid /dev/sdb
/dev/sdb: UUID="ab5b1a90-8762-48d3-a00a-46729d208c74" BLOCK_SIZE="512" TYPE="xfs"
```

#### 初始化决策盘
```bash
sh cluster_manager.sh --qdisk_init
```



#### 配置所有资源
该命令除了初始化数据库 data 目录，以及配置资源外，其他步骤都已经完成
```bash
sh cluster_manager.sh --exclude_data_init
```

#### 数据库实例初始化
该命令完成数据库初始化，并配置资源，如果 kingbase 已经完成初始化，则--init_db 不需要执行, 没有使用  
init_db 初始化数据库，则需要配置 crmsh，具体查看手工搭建集群的配置 crmsh 章节，/root/.bashrc 为环境变量  
配置参数。

```bash
source /root/.bashrc  
sh cluster_manager.sh --init_db  
sh cluster_manager.sh --config_resource
```

#### 挂载共享磁盘目录
```bash
mkdir -p /sharedata/data1/
mkdir -p /sharedata/data2/
mount /dev/sdb  /sharedata/data1
mount /dev/sdc  /sharedata/data2
```
#### 启动对应数据库


```bash
su  - kingbase
sys_ctl start -D /sharedata/data1/data
sys_ctl start -D /sharedata/data2/data
```
### node2

#### 配置cluster_manager.sh 脚本
```bash
install_dir=/opt/KingbaseHA
votingdisk=/dev/sdk
cluster_name=kcluster
use_esxi=0
use_ipmi=1
ipmi_server=(192.168.10.224 192.168.10.225)
ipmi_user=(root root)
ipmi_passwd=("kingbase.123" "kingbase.123")
##FIP
fip=(192.168.10.223 192.168.10.224)
fipinterface=ens224
netmask=24
##pacemaker
pacemaker_daemon_group=haclient
pacemaker_daemon_user=hacluster
env_bash_file=/root/.bashrc
##kingbase array should match
data_disk=("-U ab5b1a90-8762-48d3-a00a-46729d208c74" "-U 9c7d9e17-ebc5-435c-84c9-44b32ae87d9e")
data_dir=(/sharedata/data1/ /sharedata/data2/)
node_name=(node1 node2)
node_ip=(192.168.10.224 192.168.10.225)
port=(36321 36322)
fstype=xfs
kingbaseowner=kingbase
kingbasegroup=kingbase
kingbase_install_dir=/KingbaseES/V8/Server
##pingd
gateway=192.168.10.1

##crm_dsn.conf
database="test"
username="system"
# If loged in to database without password,
# the item of password could not be provided.
password="kingbase"

ping_gateway="192.168.10.1"
```
全部配置请参见 ![[cluster_manager.sh]]

```ad-warning
磁盘每次格式化后，都需要重新取UUID。
[root@node1 ~]#  blkid /dev/sdc
/dev/sdc: UUID="9c7d9e17-ebc5-435c-84c9-44b32ae87d9e" BLOCK_SIZE="512" TYPE="xfs"
[root@node1 ~]# blkid /dev/sdb
/dev/sdb: UUID="ab5b1a90-8762-48d3-a00a-46729d208c74" BLOCK_SIZE="512" TYPE="xfs"
```



#### 配置所有资源
该命令除了初始化数据库 data 目录，以及配置资源外，其他步骤都已经完成
```bash
sh cluster_manager.sh --exclude_data_init
```


#### 挂载共享磁盘目录


```bash
mkdir -p /sharedata/data1/
mkdir -p /sharedata/data2/
mount /dev/sdb  /sharedata/data1
mount /dev/sdc  /sharedata/data2
```


#### 启动node2数据库实例
```bash
su  - kingbase
sys_ctl start -D /sharedata/data1/data
sys_ctl start -D /sharedata/data2/data
```


### 查看集群状态
```bash
crm_mon
```
### 删除整个集群

```bash
sh cluster_manager.sh --clean_all
```
## 手工 搭建集群
### node1

#### 配置投票盘
1、使用`fdisk -l`查看磁盘，选择`/dev/sdk`
```bash
[root@node1 sbin]#  cd /opt/KingbaseHA/corosync-qdevice/sbin/
[root@node1 sbin]# ./mkqdisk -c /dev/sdk -l kcluster
Writing new quorum disk label 'kcluster' to /dev/sdk.
WARNING: About to destroy all data on /dev/sdk; proceed [N/y] ?
```

2、通过如下命令，可以查看共享磁盘状态。

```bash
[root@node1 sbin]# cd /opt/KingbaseHA/corosync-qdevice/sbin/
[root@node1 sbin]# ./mkqdisk -f kcluster -d
/dev/block/8:160:
/dev/disk/by-path/pci-0000:0b:00.0-scsi-0:0:10:0:
/dev/sdk:
        Magic:                eb7a62c2
        Label:                kcluster
        Created:              Fri Oct 28 18:24:36 2022
        Host:                 node1
        Kernel Sector Size:   512
        Recorded Sector Size: 512
```



#### 配置并启动corosync
##### 配置corosync.conf
```bash
cd /opt/KingbaseHA/corosync/etc/corosync  
vi corosync.conf
```
修改配置文件如下：
```
# Please read the corosync.conf.5 manual page
totem {
        version: 2

        # Set name of the cluster
        cluster_name: kcluster

        # crypto_cipher and crypto_hash: Used for mutual node authentication.
        # If you choose to enable this, then do remember to create a shared
        # secret with "corosync-keygen".
        # enabling crypto_cipher, requires also enabling of crypto_hash.
        # crypto works only with knet transport
        crypto_cipher: none
        crypto_hash: none
}

logging {
        # Log the source file and line where messages are being
        # generated. When in doubt, leave off. Potentially useful for
        # debugging.
        fileline: off
        # Log to standard error. When in doubt, set to yes. Useful when
        # running in the foreground (when invoking "corosync -f")
        to_stderr: yes
        # Log to a log file. When set to "no", the "logfile" option
        # must not be set.
        to_logfile: yes
        logfile: /opt/KingbaseHA/corosync/var/log/cluster/corosync.log
        # Log to the system log daemon. When in doubt, set to yes.
        to_syslog: yes
        # Log debug messages (very verbose). When in doubt, leave off.
        debug: off
        # Log messages with time stamps. When in doubt, set to hires (or on)
        #timestamp: hires
        logger_subsys {
                subsys: QUORUM
                debug: off
        }
}

quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        device {
        timeout: 200000
        sync_timeout: 240000
        master_wins: 1
        votes: 1
        model: disk
        disk {
                debug: 0 
                interval: 2000
                tko: 60
                tko_up: 3
                upgrade_wait: 1
                master_wait: 4
                label: kcluster
                io_timeout: 1
                fence_timeout: 5000
        }
        }
}

nodelist {
        # Change/uncomment/add node sections to match cluster configuration

        node {
                # Hostname of the node
                name: node1
                # Cluster membership node identifier
                nodeid: 1
                # Address of first link
                ring0_addr: node1
                # When knet transport is used it's possible to define up to 8 links
                #ring1_addr: 192.168.1.1
        }
        node {
                # Hostname of the node
                name: node2
                # Cluster membership node identifier
                nodeid: 2
                # Address of first link
                ring0_addr: node2
                # When knet transport is used it's possible to define up to 8 links
                #ring1_addr: 192.168.1.2
        }
        # ...
}
```



**nodelist**

| 参数       | 说明                       |
| ---------- | -------------------------- |
| name       | 节点名称。                 |
| nodeid     | 集群节点号。               |
| ring0_addr | 节点IP地址，可以填主机名。 | 

**Quorum**

| 参数  | 说明           |
| ----- | -------------- |
| label | 投票盘的标签。 | 


##### 配置启动脚本
拷贝启动脚本到系统初始化脚本目录。

```bash
cp /opt/KingbaseHA/corosync/corosync /etc/init.d/
```

##### 创建lock目录
```bash
mkdir -p /opt/KingbaseHA/corosync/var/lock/subsys
mkdir /opt/KingbaseHA/corosync/var/run
```
##### 配置corosync 日志logrotate（日志清理策略）
```bash
cp /opt/KingbaseHA/corosync/etc/logrotate.d/corosync /etc/logrotate.d/
```


##### 启动corosync
```bash
/etc/init.d/corosync start
```


#### 配置并启动pacemaker
##### 执行如下命令，创建pacemaker用户
```bash
groupadd haclient  
useradd hacluster -g haclient
```

##### 执行如下命令，添加环境变量


```bash
vi /root/.bashrc

--------------------------------------output:-------------------------------
export PATH=/opt/KingbaseHA/python2.7/bin:/opt/KingbaseHA/pacemaker/sbin:$PATH 
export PATH=/opt/KingbaseHA/crmsh/bin:/opt/KingbaseHA/pacemaker/libexec/pacemaker:$PATH 
export PATH=/opt/KingbaseHA/corosync/sbin:/opt/KingbaseHA/corosync-qdevice/sbin:$PATH
export PYTHONPATH=/opt/KingbaseHA/python2.7/lib/python2.7/site-packages/:/opt/KingbaseHA/crmsh/lib/python2.7/site-packages:$PYTHONPATH
export COROSYNC_MAIN_CONFIG_FILE=/opt/KingbaseHA/corosync/etc/corosync/corosync.conf
export CRM_CONFIG_FILE=/opt/KingbaseHA/crmsh/etc/crm/crm.conf
export OCF_ROOT=/opt/KingbaseHA/pacemaker/ocf/
export HA_SBIN_DIR=/opt/KingbaseHA/pacemaker/sbin/
```

##### 环境变量生效
```bash
source /root/.bashrc
```
##### 执行如下命令，拷贝启动脚本到系统初始化脚本位置
```bash
cp /opt/KingbaseHA/pacemaker/pacemaker /etc/init.d/
```

##### 配置pacemaker logrotate

```bash
cp /opt/KingbaseHA/pacemaker/etc/logrotate.d/pacemaker /etc/logrotate.d/
```

##### 创建lock目录
```bash
mkdir -p /opt/KingbaseHA/pacemaker/var/lock/subsys
```
##### 确保pacemaker 各个守护进程有权限对应目录。
```bash
chown -R hacluster:haclient /opt/KingbaseHA/pacemaker
```
##### 执行如下命令，启动pacemaker

```bash
/etc/init.d/pacemaker start
```
##### 查看pacemaker 日志，观察启动情况。
```bash
tail -f /opt/KingbaseHA/pacemaker/var/log/pacemaker/pacemaker.log
```

#### 启动corosync-qdevice
1、执行如下命令，拷贝 qdevie 启动脚本到系统存放初始化脚本的位置。
```bash
cp /opt/KingbaseHA/corosync-qdevice/corosync-qdevice /etc/init.d/
```
2、创建lock目录
```bash
mkdir -p /opt/KingbaseHA/corosync-qdevice/var/lock/subsys
```
3、执行如下命令，启动corosync-qdevice
```bash
/etc/init.d/corosync-qdevice start
```

4、查看启动日志：日志一般放在messages 里面，搜索关键字`corosync-qdevice`
```bash
tail -f /var/log/messages
```

5、执行如下命令，查看qdevice 状态

```bash
 /opt/KingbaseHA/corosync-qdevice/sbin/corosync-qdevice-tool -sv -p /opt/KingbaseHA/corosync-qdevice/var/run/corosync-qdevice/corosync-qdevice.sock 
```

#### 配置crmsh
1、配置 crm.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm.conf，在此配置文件中添加如下内容.
```bash
vi /opt/KingbaseHA/crmsh/etc/crm/crm.conf
--------------------------------------output:-------------------------------
[core]  
pager = less –R
```

2、配置 crm_dns.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf，其中的 password 为 base64 编码，可通过 echo -n‘123456’| base64 得到编码后的密码
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^wgnrohe7vzf\|配置 crm_dns.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf，其中的 password 为 base64 编码，可通过 echo -n‘123456’]]
```bash
vi /opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf
--------------------------------------output:-------------------------------
[kingbase resource name1]
username=system
password=a2luZ2Jhc2U=
port=54321
database=test

[kingbase resource name2]
username=system
password=a2luZ2Jhc2U=
port=54321
database=test
```
### node2

#### 查看投票盘
1、通过如下命令，可以查看共享磁盘状态。

```bash
[root@node1 sbin]# cd /opt/KingbaseHA/corosync-qdevice/sbin/
[root@node1 sbin]# ./mkqdisk -f kcluster -d
/dev/block/8:160:
/dev/disk/by-path/pci-0000:0b:00.0-scsi-0:0:10:0:
/dev/sdk:
        Magic:                eb7a62c2
        Label:                kcluster
        Created:              Fri Oct 28 18:24:36 2022
        Host:                 node1
        Kernel Sector Size:   512
        Recorded Sector Size: 512
```



#### 配置并启动corosync
##### 配置corosync.conf
```bash
cd /opt/KingbaseHA/corosync/etc/corosync  
vi corosync.conf
```
修改配置文件如下：
```
# Please read the corosync.conf.5 manual page
totem {
        version: 2

        # Set name of the cluster
        cluster_name: kcluster

        # crypto_cipher and crypto_hash: Used for mutual node authentication.
        # If you choose to enable this, then do remember to create a shared
        # secret with "corosync-keygen".
        # enabling crypto_cipher, requires also enabling of crypto_hash.
        # crypto works only with knet transport
        crypto_cipher: none
        crypto_hash: none
}

logging {
        # Log the source file and line where messages are being
        # generated. When in doubt, leave off. Potentially useful for
        # debugging.
        fileline: off
        # Log to standard error. When in doubt, set to yes. Useful when
        # running in the foreground (when invoking "corosync -f")
        to_stderr: yes
        # Log to a log file. When set to "no", the "logfile" option
        # must not be set.
        to_logfile: yes
        logfile: /opt/KingbaseHA/corosync/var/log/cluster/corosync.log
        # Log to the system log daemon. When in doubt, set to yes.
        to_syslog: yes
        # Log debug messages (very verbose). When in doubt, leave off.
        debug: off
        # Log messages with time stamps. When in doubt, set to hires (or on)
        #timestamp: hires
        logger_subsys {
                subsys: QUORUM
                debug: off
        }
}

quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        device {
        timeout: 200000
        sync_timeout: 240000
        master_wins: 1
        votes: 1
        model: disk
        disk {
                debug: 0 
                interval: 2000
                tko: 60
                tko_up: 3
                upgrade_wait: 1
                master_wait: 4
                label: kcluster
                io_timeout: 1
                fence_timeout: 5000
        }
        }
}

nodelist {
        # Change/uncomment/add node sections to match cluster configuration

        node {
                # Hostname of the node
                name: node1
                # Cluster membership node identifier
                nodeid: 1
                # Address of first link
                ring0_addr: node1
                # When knet transport is used it's possible to define up to 8 links
                #ring1_addr: 192.168.1.1
        }
        node {
                # Hostname of the node
                name: node2
                # Cluster membership node identifier
                nodeid: 2
                # Address of first link
                ring0_addr: node2
                # When knet transport is used it's possible to define up to 8 links
                #ring1_addr: 192.168.1.2
        }
        # ...
}
```



**nodelist**

| 参数       | 说明                       |
| ---------- | -------------------------- |
| name       | 节点名称。                 |
| nodeid     | 集群节点号。               |
| ring0_addr | 节点IP地址，可以填主机名。 | 

**Quorum**

| 参数  | 说明           |
| ----- | -------------- |
| label | 投票盘的标签。 | 


##### 配置启动脚本
拷贝启动脚本到系统初始化脚本目录。

```bash
cp /opt/KingbaseHA/corosync/corosync /etc/init.d/
```

##### 创建lock目录
```bash
mkdir -p /opt/KingbaseHA/corosync/var/lock/subsys
mkdir /opt/KingbaseHA/corosync/var/run
```
##### 配置corosync 日志logrotate（日志清理策略）
```bash
cp /opt/KingbaseHA/corosync/etc/logrotate.d/corosync /etc/logrotate.d/
```


##### 启动corosync
```bash
/etc/init.d/corosync start
```


#### 配置并启动pacemaker
##### 执行如下命令，创建pacemaker用户
```bash
groupadd haclient  
useradd hacluster -g haclient
```

##### 执行如下命令，添加环境变量


```bash
vi /root/.bashrc

--------------------------------------output:-------------------------------
export PATH=/opt/KingbaseHA/python2.7/bin:/opt/KingbaseHA/pacemaker/sbin:$PATH 
export PATH=/opt/KingbaseHA/crmsh/bin:/opt/KingbaseHA/pacemaker/libexec/pacemaker:$PATH 
export PATH=/opt/KingbaseHA/corosync/sbin:/opt/KingbaseHA/corosync-qdevice/sbin:$PATH
export PYTHONPATH=/opt/KingbaseHA/python2.7/lib/python2.7/site-packages/:/opt/KingbaseHA/crmsh/lib/python2.7/site-packages:$PYTHONPATH
export COROSYNC_MAIN_CONFIG_FILE=/opt/KingbaseHA/corosync/etc/corosync/corosync.conf
export CRM_CONFIG_FILE=/opt/KingbaseHA/crmsh/etc/crm/crm.conf
export OCF_ROOT=/opt/KingbaseHA/pacemaker/ocf/
export HA_SBIN_DIR=/opt/KingbaseHA/pacemaker/sbin/
```

##### 环境变量生效
```bash
source /root/.bashrc
```
##### 执行如下命令，拷贝启动脚本到系统初始化脚本位置
```bash
cp /opt/KingbaseHA/pacemaker/pacemaker /etc/init.d/
```

##### 配置pacemaker logrotate

```bash
cp /opt/KingbaseHA/pacemaker/etc/logrotate.d/pacemaker /etc/logrotate.d/
```

##### 创建lock目录
```bash
mkdir -p /opt/KingbaseHA/pacemaker/var/lock/subsys
```
##### 确保pacemaker 各个守护进程有权限对应目录。
```bash
chown -R hacluster:haclient /opt/KingbaseHA/pacemaker
```
##### 执行如下命令，启动pacemaker

```bash
/etc/init.d/pacemaker start
```
##### 查看pacemaker 日志，观察启动情况。
```bash
tail -f /opt/KingbaseHA/pacemaker/var/log/pacemaker/pacemaker.log
```

#### 启动corosync-qdevice
1、执行如下命令，拷贝 qdevie 启动脚本到系统存放初始化脚本的位置。
```bash
cp /opt/KingbaseHA/corosync-qdevice/corosync-qdevice /etc/init.d/
```
2、创建lock目录
```bash
mkdir -p /opt/KingbaseHA/corosync-qdevice/var/lock/subsys
```
3、执行如下命令，启动corosync-qdevice
```bash
/etc/init.d/corosync-qdevice start
```

4、查看启动日志：日志一般放在messages 里面，搜索关键字`corosync-qdevice`
```bash
tail -f /var/log/messages
```

5、执行如下命令，查看qdevice 状态

```bash
 /opt/KingbaseHA/corosync-qdevice/sbin/corosync-qdevice-tool -sv -p /opt/KingbaseHA/corosync-qdevice/var/run/corosync-qdevice/corosync-qdevice.sock 
```

#### 配置crmsh
1、配置 crm.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm.conf，在此配置文件中添加如下内容.
```bash
vi /opt/KingbaseHA/crmsh/etc/crm/crm.conf
--------------------------------------output:-------------------------------
[core]  
pager = less –R
```

2、配置 crm_dns.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf，其中的 password 为 base64 编码，可通过 echo -n‘123456’| base64 得到编码后的密码
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^wgnrohe7vzf\|配置 crm_dns.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf，其中的 password 为 base64 编码，可通过 echo -n‘123456’]]
```bash
vi /opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf
--------------------------------------output:-------------------------------
[kingbase resource name1]
username=system
password=a2luZ2Jhc2U=
port=54321
database=test

[kingbase resource name2]
username=system
password=a2luZ2Jhc2U=
port=54321
database=test
```
### crm 配置资源

```ad-warning
1、以下操作，只需在某一台机器上执行

```
#### 配置fenceing1 资源
针对于fence 配置有两种模式：
1、VMWARE ESXI 环境。
2、IPMI 环境。
本次实验使用IPMI 环境。

1、[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^2z8ptqvo0bn\|针对ipmilan 环境配置fence，需要注意IPMI 设备一般为各个节点独占，因此需要为各个节点配置单独的fence资源。]]

```bash
crm configure primitive fence_node1_ipmi stonith:fence_ipmilan params ipaddr=node1 login=root  passwd="kingbase.123" pcmk_host_list=node1  lanplus=true ipmitool_path=/opt/KingbaseHA/ipmi_tool/bin/ipmitool op monitor interval=60s meta failure-timeout=5min
```

#### 配置fenceing2 资源
```bash
crm configure primitive fence_node2_ipmi stonith:fence_ipmilan params ipaddr=node2 login=root  passwd="kingbase.123" pcmk_host_list=node2  lanplus=true ipmitool_path=/opt/KingbaseHA/ipmi_tool/bin/ipmitool op monitor interval=60s meta failure-timeout=5min
```

```ad-warning
如果配置有误，请使用以下命令删除对应fence 资源。
crm configure delete fence_node1_ipmi
```

2、[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^frvdb9o3sam\|执行如下命令，添加 fence_node1_ipmi 和 fence_node2_ipmi 的 location 约束。]]

[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^77th3c22mc3\|为了保证 fence_node1_ipmi资源尽量不在节点 node1，fence_node2_ipmi 资源尽量不在节点 node2，从而减少 fence 自己的概率，又由于资源在各个节点的默认分数为 0，因此需要保证 fence_node1_ipmi 资源在 node2 的分数、fence_node2_ipmi在 node1 的分数均大于 rsc_defaults resource-stickiness 的分数]]

```bash
crm configure location fence_node1_ipmi-on-node2 fence_node1_ipmi 1000: node2  
crm configure location fence_node2_ipmi-on-node1 fence_node2_ipmi 1000: node1
```

```ad-warning
这是啥意思
？

```


3、测试是否能够重启
```bash
fence_ipmilan -a 192.168.10.225 -l root -p 'kingbase.123' -P  --ipmitool-path=/opt/KingbaseHA/ipmi_tool/bin/ipmitool -o reboot
```
```ad-warning
测试未成功，返回错误：`2022-10-31 13:23:34,476 ERROR: Connection timed out`
```
#### 添加VIP 1资源
```bash
crm configure primitive FIP0 ocf:heartbeat:IPaddr2 \
params ip="192.168.10.223" cidr_netmask="24" nic="ens224" \
arp_sender="iputils_arping" \
op monitor interval="30s" timeout="60s" \
op start interval="0" timeout="60s" \
op stop interval="0" timeout="60s" \
meta failure-timeout=5min
```
#### 添加VIP 2资源
```bash
crm configure primitive FIP2 ocf:heartbeat:IPaddr2 \
params ip="192.168.4.226" cidr_netmask="24" nic="ens224" \
arp_sender="iputils_arping" \
op monitor interval="30s" timeout="60s" \
op start interval="0" timeout="60s" \
op stop interval="0" timeout="60s" \
meta failure-timeout=5min
```


#### 执行如下命令，添加 VIP1 location 约束, 多个节点，最好分数不一样
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^puywlk0fcpb\|执行如下命令，添加 VIP1 location 约束, 多个节点，最好分数不一样]]
```bash
crm configure location FIP1-on-node1 FIP1 1000: node1
crm configure location FIP1-on-node2 FIP1 1000: node2
```



#### 添加FILESYSTEM1 资源。
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^5ev0m9c8nka\|执行如下命令，添加 FILESYSTEM1 资源，需确保每个节点/sharedata/data1 目录存在，sharedata 属主是root，data1 属主改成数据库用户。使用如下命令查看存储设备/dev/sdc 对应的 uuid]]
1、添加FILESYSTEM1文件系统


```bash

mkdir -p /sharedata/data1
chown kingbase:kingbase /sharedata/data1
[root@node1 ~]# blkid /dev/sdb
--------------------------------------output:-------------------------------
/dev/sdb: UUID="345f26e2-4e52-4efb-97ba-1e276f9fda40" BLOCK_SIZE="512" TYPE="xfs"
[root@node1 ~]# blkid /dev/sdc
--------------------------------------output:-------------------------------
/dev/sdc: UUID="28eb5766-ce3d-4306-b91a-1c85b373f414" BLOCK_SIZE="512" TYPE="xfs"
```

2、添加资源
```bash
crm configure primitive FILESYSTEM1 ocf:heartbeat:Filesystem \
params device="-U 345f26e2-4e52-4efb-97ba-1e276f9fda40" \
directory="/sharedata/data1" fstype="xfs" \
op start interval="0" timeout="60" \
op stop interval="0" timeout="60" \
op monitor interval="30s" timeout="60" OCF_CHECK_LEVEL="20" \
meta failure-timeout=5min
```

#### 添加FILESYSTEM2资源。


```bash
crm configure primitive FILESYSTEM2 ocf:heartbeat:Filesystem \
params device="-U 28eb5766-ce3d-4306-b91a-1c85b373f414" \
directory="/sharedata/data1" fstype="xfs" \
op start interval="0" timeout="60" \
op stop interval="0" timeout="60" \
op monitor interval="30s" timeout="60" OCF_CHECK_LEVEL="20" \
meta failure-timeout=5min
```


4、[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^vpfgtdn0qu\|执行如下命令，添加 PINGD 资源. 目前来说，当网关与集群网络心跳共用同一个网络时，发生网络故障时，将会触发分区故障，而我们总是选择最小的 ID 当主节点，此时如果主节点刚好接口 down 掉，此时集群整个处于不能对外提供服务状态，此时考虑可以设置 failure_action=”reboot”,failure_retries 参数，当集群不能对外提供服务时，自身 reboot，剩余的节点对外提供服务，后面基于 heuristics 测试选主做好之后，可以避免这种情况]]

```bash
crm configure primitive PINGD ocf:pacemaker:ping \
params host_list="192.168.10.1" multiplier="100" name="ping-in" \
failure_score="100" failure_action="reboot" failure_retries=3 \
op monitor interval="2" timeout="90" \
op start interval="0" timeout="90" \
op stop interval="0" timeout="90" \
meta failure-timeout=5min
```
5、[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^yh0860xryqd\|执行如下命令，将 PIND 资源，变成 clone 资源]]

```bash
crm configure clone CLONE-PINGD PINGD
```
#### 添加DB1资源
```ad-warning

这里所有的数据库命令全部要替换。
```

```
crm configure primitive DB1 ocf:kingbase:kingbase \
params sys_ctl="/home/kingbase/V8/Server/bin/sys_ctl" \
ksql="/home/kingbase/V8/Server/bin/ksql" \
sys_isready="/home/kingbase/V8/Server/bin/sys_isready" \
kb_data="/sharedata/data1/data" \
kb_dba="kingbase" kb_host="0.0.0.0" \
kb_user="system" \
kb_port="54321" \
kb_libs="/home/kingbase/V8/Server/lib" \
kb_db="template1" \
logfile="/home/kingbase/V8/Server/log/kingbase2.log" \
op start interval="0" timeout="120" \
op stop interval="0" timeout="120" \
op monitor interval="9s" timeout="30" \
meta failure-timeout=5min
```
#### 添加DB2资源
```
crm configure primitive DB2 ocf:kingbase:kingbase \
params sys_ctl="/home/kingbase/V8/Server/bin/sys_ctl" \
ksql="/home/kingbase/V8/Server/bin/ksql" \
sys_isready="/home/kingbase/V8/Server/bin/sys_isready" \
kb_data="/sharedata/data2/data" \
kb_dba="kingbase" kb_host="0.0.0.0" \
kb_user="system" \
kb_port="54322" \
kb_libs="/home/kingbase/V8/Server/lib" \
kb_db="template1" \
logfile="/home/kingbase/V8/Server/log/kingbase2.log" \
op start interval="0" timeout="120" \
op stop interval="0" timeout="120" \
op monitor interval="9s" timeout="30" \
meta failure-timeout=5min
```
#### 创建DB1 资源组
把对应VIP 文件系统资源绑定在一起。
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^j2fm3f8mtd\|执行如下命令，创建 DB1 组资源]]
```bash
[root@node1 ~]# crm configure group DB1_GROUP FIP1 FILESYSTEM1 DB1
```
#### 创建DB2资源组
把对应VIP 文件系统资源绑定在一起。
[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^j2fm3f8mtd\|执行如下命令，创建 DB1 组资源]]
```bash
[root@node1 ~]# crm configure group DB2_GROUP FIP2 FILESYSTEM2 DB2
```
#### 配置DB1负载
```bash
[root@node1 ~]# crm configure location DB1-on-node1 DB1_GROUP 1000: node1
[root@node1 ~]# crm configure location DB1-on-node2 DB1_GROUP 800: node2
```
#### 配置DB2负载
```bash
[root@node1 ~]# crm configure location DB2-on-node1 DB1_GROUP 1000: node1
[root@node1 ~]# crm configure location DB2-on-node2 DB1_GROUP 800: node2
```


#### 查看资源配置信息
 ```bash
crm configure show
```