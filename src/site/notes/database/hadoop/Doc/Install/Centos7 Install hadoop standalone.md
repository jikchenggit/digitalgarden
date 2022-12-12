---
{"author":"aming","email":"jikcheng@163.com","title":"Centos7 Install hadoop standalone","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"Centos7 Install hadoop standalone","File Folder with relative path":"database/hadoop/Doc/Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/hadoop/doc/install/centos7-install-hadoop-standalone/","dgPassFrontmatter":true}
---



## 2. Hadoop: 设置单个节点集群
### 2.1. 先决条件
#### 2.1.1. 支持的平台
Hadoop 支持GNU/Linux平台.和Windows 平台.
#### 2.1.2. 软件基础环境
- java 是hadoop 的必备组件,具体支持的java 版本请参见[hadoopjavaversion](http://wiki.apache.org/hadoop/HadoopJavaVersions)
- 使用启动和停止脚本,必须安装ssh 和sshd 管理hadoop 脚本.
#### 2.1.3. 安装软件
```bash
yum install ssh
```
#### 2.1.4. 下载软件
下载需要的版本,具体链接[hadoopdown](http://www.apache.org/dyn/closer.cgi/hadoop/common/)

### 2.2. 准备启动hadoop 集群
解压下载的hadoop 软件.在发布软件编辑软件中 `etc/hadoop/hadoop-env.sh ` 定义参数.
```bash
 # set to the root of your Java installation
  export JAVA_HOME=/usr/java/latest
```
### 2.3. 测试
```bash
$ bin/hadoop
```
将会显示hadoop 的使用文档.
启动方式有三种:
	 - 本地模式(Local (Standalone) Mode )
   - 伪分布模式(Pseudo-Distributed Mode)
   - 全分布模式(Fully-Distributed Mode)
### 2.4. 本地模式操作
默认情况下,Hadoop 被配置为作为单 Java 进程以非分布式模式运行.对于调试非常有用. 
以下示例将conf 目录下的所有文件作为输入,然后查找并显示给定正则表达式的匹配项.输出将会输出到指定的输出目录.
```bash
  $ mkdir input
  $ cp etc/hadoop/*.xml input
  $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'
  $ cat output/*
```
### 2.5. 伪分布模式
Hadoop 可以在单节点删以伪分布模式运行,每个Hadoop 守护进程运行在单独的java 进程中.
#### 2.5.1. 配置
- etc/hadoop/core-site.xml:
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://192.168.10.170:9000</value>
    </property>
</configuration>
```
- etc/hadoop/hdfs-site.xml:

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```
#### 2.5.2. 设置ssh 免密登录
可以检查你的ssh 主机有没有密码:
```bash
$ ssh hadoop
```
设置免密登录
```bash
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys
```
### 2.6. 执行
下面说明了在本地运行`MapReduce` 作业.
1. **格式化文件系统**
```bash
$ bin/hdfs namenode -format
```
2.  **启动namenode 和dataone 进程**

```bash
$ sbin/start-dfs.sh
```
3. **浏览namenode 站点**
```
NameNode - http://localhost:9870/
```
4. 创建HDFS 目录执行`MapReduce` 作业
```
$ bin/hdfs dfs -mkdir /user
$ bin/hdfs dfs -mkdir /user/<username>
```
5. **将输入文件复制到分布式文件系统**
```
$ bin/hdfs dfs -mkdir input
$ bin/hdfs dfs -put etc/hadoop/*.xml input
```
## 3. YARN 单点守护配置
下面说明为示例，1-4 步骤。

配置以下参数：

etc/hadoop/mapred-site.xml:

```html
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```
etc/hadoop/yarn-site.xml:

```html
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

启动资源管理器和节点管理器守护进程。

```bash
$ sbin/start-yarn.sh
```

使用web 浏览器 ， 访问资源管理器页面。
```bash
ResourceManager - http://localhost:8088/
```

运行MapReduce 。


验证完毕后关闭yarn 程序。

```bash
$ sbin/stop-yarn.sh
```