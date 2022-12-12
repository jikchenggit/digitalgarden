---
{"author":"aming","email":"jikcheng@163.com","title":"Centos 7  Install Hadoop Cluster","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"Centos 7  Install Hadoop Cluster","File Folder with relative path":"database/hadoop/Doc/Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/hadoop/doc/install/centos-7-install-hadoop-cluster/","dgPassFrontmatter":true}
---



### 1.1. 介绍
本文档描述了如何安装和配置hadoop 集群，范围从几个节点到数千个节点集群。要使用Hadoop。首先想要在一台机器上安装（参见单节点安装[安装hadoop 到centos7 standalone.md](file:///D:/SynologyDrive/vnote_notebooks/hadoop/安装/安装hadoop%20到centos7%20standalone.md)）
此文档不涉及安全性和高可用等高级主题。
### 1.2. 软件基础环境
- java 是hadoop 的必备组件,具体支持的java 版本请参见[hadoopjavaversion](http://wiki.apache.org/hadoop/HadoopJavaVersions)
- 使用启动和停止脚本,必须安装ssh 和sshd 管理hadoop 脚本.
## 2. 安装
安装Hadoop 集群通常涉及在集群中所有的机器上解压软件，将硬件划分为不同的功能是很重要的。
通常，集群中的一台机器指定为`NameNode` ,另一台机器被指定为`ResourceManager` .这些是管理节点.其他服务(如web app 代理服务器和`MapReduce` 作业历史记录服务器) 通常在专用硬件和共享基础设施运行,具体取决于业务负载大小.
### 2.1. 配置hadoop 运行为非安全模式
hadoop 的Java 配置是由两类重要的配置文件类型:
- 只读默认配置 :`-core-default.xml`, `hdfs-default.xml`, `yarn-default.xml` 和`mapred-default.xml`.
- 用于特定站点的配置: ` etc/hadoop/core-site.xml`, `etc/hadoop/hdfs-site.xml`, `etc/hadoop/yarn-site.xml` 和`etc/hadoop/mapred-site.xml`.

对于bin/ 目录下找到的Hadoop 脚本,可以使用 `etc/hadoop/hadoop-env.sh` 和 `etc/hadoop/yarn-ens.sh`

要配置Hadoop 集群,需要配置Hadoop 守护进程执行的环境以及Hadoop 守护进程的配置参数.

要配置Hadoop 集群,需要配置Hadoop 守护进程的环境,和守护进程的配置参数.

HDFS 守护进程是`NameNode`,`SecondaryNameNode` 和`DataNode` .YARN 进程为`ResourceManager` ,`NodeManager` 和`WebAppProxy` .如果要使用`MapReduce` ,那么`MapReduce` job 的记录服务器也会运行,对于集群性安装,通常在单独的主机删运行.配置Hadoop 守护环境可以使用`etc/hadoop/haoop-env.sh` 和`etc/hadoop/yarn-env.sh` 脚本环境变量进行控制.

### 2.2. 配置hadoop 守护进程的环境信息
管理员使用`etc/hadoop/hadoop-env.sh` 也可以使用`etc/hadoop/mapred-env.sh` 和`etc/hadoop/yarn-env.sh` 脚本对hadoop 守护进程 的环境进行定制,必须指定JAVA_HOME ,一遍在每个远程节点删正确的定义它.

管理员可以使用以下表示的配置选项配置各个守护进程:

| Daemon                        | Environment Variable        |
| :---------------------------- | :-------------------------- |
| NameNode                      | HDFS_NAMENODE_OPTS          |
| DataNode                      | HDFS_DATANODE_OPTS          |
| Secondary NameNode            | HDFS_SECONDARYNAMENODE_OPTS |
| ResourceManager               | YARN_RESOURCEMANAGER_OPTS   |
| NodeManager                   | YARN_NODEMANAGER_OPTS       |
| WebAppProxy                   | YARN_PROXYSERVER_OPTS       |
| Map Reduce Job History Server | MAPRED_HISTORYSERVER_OPTS   |

例如: 要配置Namenode 来使用parallelGC 和4GB java 堆栈,应该在hadoop-env.sh 中添加以下语句:
```bash
 export HDFS_NAMENODE_OPTS="-XX:+UseParallelGC -Xmx4g"
```

其他例子:请见`etc/hadoop/hadoop-env.sh` 
其他优用的配置参数,可以自定义包括:
HADOOP_PID_DIR - 守护进程的PID 文件存储目录.
HADOOP_LOG_DIR - 守护进程的日志文件存储目录.如果日志不存在,自动创建他们.

HADOOP_HEAPSIZE_MAX - 用户Java 堆大小的最大内存量.支持JVM 党员. 默认为MB 为单位.

在大多数情况下,您应该指定`HADOOP_PID_DIR` 和`HADOOP_LOG_DIR` 目录,这样只能由hadoop 的守护进程的用户写入,否则可能会发生符号链接攻击.

在系统曾联配置HADOOP_HOME 也是一种习惯, 例如:
```bash
 HADOOP_HOME=/path/to/hadoop
  export HADOOP_HOME
```
### 2.3. 配置hadoop 守护进程参数
讨论在特定的配置文件中指定重要的参数:

- etc/hadoop/core-site.xml

| Parameter           | Value        | Notes                                                                                     |
| :------------------ | :----------- | :---------------------------------------------------------------------------------------- |
| fs.defaultFS        | NameNode URI | [hdfs://host:port/](hdfs://host:port/)                                                    |
| io.file.buffer.size | 131072       | 顺序读/写文件中的大小,决定了数据在读写操作期间的缓冲大小.影响读写速率   (必须是硬件扇区的整数倍) |

例子:
```html
<property>
            <name>fs.defaultFS</name>
            <value>hdfs://192.168.10.170:9000</value>
            <name>io.file.buffer.size</name>
            <value>131072</value>
</property>
```

- etc/hadoop/hdfs-site.xml
NameNode 配置

| Parameter                     | Value                                                                         | Notes                                                                       |
| :---------------------------- | :---------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| dfs.namenode.name.dir         | NameNode在本地文件系统存储的命名空间和事务日志的路径,                            | 如果是一个以逗号分割符的目录列表,将在所有的目录中复制name表,进行冗余             |
| dfs.hosts / dfs.hosts.exclude | 允许/不允许的datanode 列表.                                                    | 如果需要,可以使用此文件控制datanode 列表.                                     |
| dfs.blocksize                 | 新文件的默认块大小,以字节为单位.可以使用:k,m,g,t,p,e 指定大小,或者使用默认字节单位 | 大文件系统的HDfS 块大小为256MB.                                               |
| dfs.namenode.handler.count    | 100                                                                           | namenode 的数据服务RPC线程,用来处理客户端请求.如果没有配置,则监听所有节点的请求. |

DataNode的配置:

| Parameter             | Value                                                                  | Notes                                                                                    |
| :-------------------- | :--------------------------------------------------------------------- | :--------------------------------------------------------------------------------------- |
| dfs.datanode.data.dir | 在DataNode本地文件系统删用逗号分隔符路径列表,DataNode 应该在这里存储数据块 | 如果是一个逗号分隔的目录列表,那么数据将存储在所有的命名目录中,通常不同的目录挂载到不同的设备上. |

- etc/hadoop/yarn-site.xml
`ResourceManager` 和`NodeManager` 的配置:

| Parameter                   | Value        | Notes                                                                                                        |
| :-------------------------- | :----------- | :----------------------------------------------------------------------------------------------------------- |
| yarn.acl.enable             | true / false | 是否开启ACL 认证? 默认为false.                                                                                 |
| yarn.admin.acl              | Admin ACL    | 在集群上设置管理员的ACL .ACL 适用于逗号分隔的用户空间分隔的组,默认为 * ,表示任何人都可以访问,空格表示没有人可以访问. |
| yarn.log-aggregation-enable | *false*      | 启用或禁用日志聚合的配置.                                                                                      |

ResourceManager 配置:

| Parameter                                                                         | Value                                                                                   | Notes                                                                                                                                              |
| :-------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| yarn.resourcemanager.address                                                      | 资源管理器的 host:port 用于客户端提交job 的端口.                                           | 如果host:port 设置后, 则覆盖`yarn.resourcemanager.hostname` 中设置主机名.                                                                            |
| yarn.resourcemanager.scheduler.address                                            | ResourceManager host:port  调度程序和资源获取的端口.                                      | 如果host:port 设置后, 则覆盖`yarn.resourcemanager.hostname` 中设置主机名.                                                                            |
| yarn.resourcemanager.resource-tracker.address                                     | ResourceManager host:port 节点管理器的端口.                                               | 如果host:port 设置后, 则覆盖`yarn.resourcemanager.hostname` 中设置主机名.                                                                            |
| yarn.resourcemanager.admin.address                                                | ResourceManager host:port for 管理命令的端口.                                            | 如果host:port 设置后, 则覆盖`yarn.resourcemanager.hostname` 中设置主机名.                                                                            |
| yarn.resourcemanager.webapp.address                                               | ResourceManager  host:port webUI 的端口.                                                 | 如果host:port 设置后, 则覆盖`yarn.resourcemanager.hostname` 中设置主机名.                                                                            |
| yarn.resourcemanager.hostname                                                     | ResourceManager                                                                     主机名 | 只设置主机名,可以进行默认端口设置.                                                                                                                   |
| yarn.resourcemanager.scheduler.class                                              | ResourceManager 调度器类.                                                                | capacity 调度(推荐) ,FairScheduler(推荐) 或fifoschscheduler(推荐) e.g., org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler. |
| yarn.scheduler.minimum-allocation-mb                                              | 在资源管理器上为每个容器请求分配最小的内存限制.                                             | In MBs                                                                                                                                             |
| yarn.scheduler.maximum-allocation-mb                                              | 在资源管理上为每个容器请求分配最大内存限制                                                  | In MBs                                                                                                                                             |
| yarn.resourcemanager.nodes.include-path / yarn.resourcemanager.nodes.exclude-path | 允许/不允许的路径名单                                                                     | 如果需要,可以使用这些文件控制节点管理列表.                                                                                                            |

NodeManager 配置:

| Parameter                                  | Value                                                                                     | Notes                                                                                                                                                                                                                                                                |
| :----------------------------------------- | :---------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| yarn.nodemanager.resource.memory-mb        | 可以分配物理内存总量(以MB为单位).                                                           | 如果设为-1 和探测硬件能里是真,在其他情况为8192MB.                                                                                                                                                                                                                       |
| yarn.nodemanager.vmem-pmem-ratio           | Maximum ratio by which virtual memory usage of tasks may exceed physical memory           | The virtual memory usage of each task may exceed its physical memory limit by this ratio. The total amount of virtual memory used by tasks on the NodeManager may exceed its physical memory usage by this ratio.                                                    |
| yarn.nodemanager.local-dirs                | Comma-separated list of paths on the local filesystem where intermediate data is written. | Multiple paths help spread disk i/o.                                                                                                                                                                                                                                 |
| yarn.nodemanager.log-dirs                  | Comma-separated list of paths on the local filesystem where logs are written.             | Multiple paths help spread disk i/o.                                                                                                                                                                                                                                 |
| yarn.nodemanager.log.retain-seconds        | *10800*                                                                                   | Default time (in seconds) to retain log files on the NodeManager Only applicable if log-aggregation is disabled.                                                                                                                                                     |
| yarn.nodemanager.remote-app-log-dir        | */logs*                                                                                   | HDFS directory where the application logs are moved on application completion. Need to set appropriate permissions. Only applicable if log-aggregation is enabled.                                                                                                   |
| yarn.nodemanager.remote-app-log-dir-suffix | *logs*                                                                                    | Suffix appended to the remote log dir. Logs will be aggregated to ${yarn.nodemanager.remote-app-log-dir}/${user}/${thisParam} Only applicable if log-aggregation is enabled.                                                                                         |
| yarn.nodemanager.aux-services              | mapreduce_shuffle                                                                         | Shuffle service that needs to be set for Map Reduce applications.                                                                                                                                                                                                    |
| yarn.nodemanager.env-whitelist             | Environment properties to be inherited by containers from NodeManagers                    | For mapreduce application in addition to the default values HADOOP\_MAPRED\_HOME should to be added. Property value should JAVA\_HOME,HADOOP\_COMMON\_HOME,HADOOP\_HDFS\_HOME,HADOOP\_CONF\_DIR,CLASSPATH\_PREPEND\_DISTCACHE,HADOOP\_YARN\_HOME,HADOOP\_MAPRED_HOME |