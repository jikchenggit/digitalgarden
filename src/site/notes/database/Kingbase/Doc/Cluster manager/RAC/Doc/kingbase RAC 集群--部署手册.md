---
{"annotation-target":"https://www.aming.work:8083/download/22121/pdf/22121.pdf","annotation-target-type":"pdf","author":"aming","email":"jikcheng@163.com","title":"kingbase RAC 集群--部署手册","creation_date":"2022-10-27 10:46","Last modified date":"2022-12-25 23:01","tags":"kingbase RAC 集群--部署手册","File Folder with relative path":"database/Kingbase/Doc/Cluster manager/RAC/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/kingbase/doc/cluster-manager/rac/doc/kingbase-rac/","dgPassFrontmatter":true}
---








>
>*共享的存储设备。基于共享存储，Corosync-qdevice ==进程会进行选举，实现分区quorum 的判定。== 在选举中赢得选举的分区，Corosync 之间会达成共识，实现集*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^ksnmtgryz3a\|show annotation]]
>
>
>
>
^ksnmtgryz3a


>
>*节点数据共享的功能，需要提供多节点共享的存储设备。基于共享存储， ==Corosync-qdevice 进程会进行选举，实现分区quorum 的判定。== 在选举中赢得选举的分区，Corosync 之间会达成共识，实现集*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^txe79dkk8sc\|show annotation]]
>
>
>
>
^txe79dkk8sc


>
>*节点数据共享的功能，需要提供多节点共享的存储设备。基于共享存储， ==Corosync-qdevice 进程会进行选举，实现分区quorum 的判定。在选举中赢得选举的分区，Corosync 之间会达成共识，实现集群的通信功能。这样当我们通过crmsh 配置Pacemaker 管理的资源时，配置信息会通过Corosync 广播到整个集群。除了配置的广播，Corosync 还会实现运行中各种状态和数据的通信，是 Clusterware 的通信子系统。== 同时由于 corosync-qdevice 也会通过 coros*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^poi45t6nvd\|show annotation]]
>
>
>
>
^poi45t6nvd


>
>*括 qdevice）启动后，需要拉起 Pacemaker 服务。 ==Pacemaker 主要负责管理外部资源，包括 VIP、Filesystem 以及分库资源。通过 crmsh可以有效的操作 Pacemaker 按照各种规则和约束去管理各类资源。实际上，crmsh 是管理员（DBA）最常用的 Clus-terware 工具，是管理员配置和管理 Pacemaker 以及 Pacemaker 管理的资源的重要手段。== 1) KingbaseES clusterware 典型组网方式*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^4m1lh19t88m\|show annotation]]
>
>
>
>
^4m1lh19t88m


>
>*解决大部分分布式系统中出现的问题，如脑裂、集群节点的增减等问题。 ==作为去中心化的系统，避免了中心节点由于集群过大带来的过多负载压力的问题，在大集群应用的情况能够很好的提供集群系统的高吞吐、高压力、高负载的承载。== 2.2 系统架构Clusterware 主要提供集群管理服务功能*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^gz4rq05fxxj\|show annotation]]
>
>
>
>
^gz4rq05fxxj


>
>*章 系统结构本章节包含以下内容：• 概述• 系统架构2.1 概述 ==Clusterware 作为全局资源管理的系统，可以解决大部分分布式系统中出现的问题，如脑裂、集群节点的增减等问题。== 作为去中心化的系统，避免了中心节点由于集群过大带来的过多负载压力*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^h3ombhc8j6t\|show annotation]]
>
>
>
>
^h3ombhc8j6t


>
>*结构2) KingbaseES clusterware 运行架构 ==组件中 pacemaker、corosync（包含 qdevice）都有 daemon 运行在每个节点上。resource agents、crmsh 没有daemon 运行，会在执行的节点上运行。== 5第 3 章 集群搭建前置操作第3章 集群搭建前置操作搭建集群中*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^mjqkb1zvf9\|show annotation]]
>
>
>
>
^mjqkb1zvf9


>
>*好的提供集群系统的高吞吐、高压力、高负载的承载。2.2 系统架构 ==Clusterware 主要提供集群管理服务功能，在部署时，需要将Clusterware 部署在集群中的每个节点上。为了实现多节点数据共享的功能，需要提供多节点共享的存储设备。基于共享存储== ，Corosync-qdevice 进程会进行选举，实现分区qu*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^s180yyhrtb\|show annotation]]
>
>
>
>
^s180yyhrtb


>
>*cemaker 以及 Pacemaker 管理的资源的重要手段。 ==1) KingbaseES clusterware 典型组网方式如下图：== 4第 2 章 系统结构2) KingbaseES cluster*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^fhzhefyp9tc\|show annotation]]
>
>
>
>
^fhzhefyp9tc


>
>*点可见存储设备的配置跨节点使用存储设备，除 DAS 方式外，还有 ==虚拟机共享虚拟磁盘== 的方式。在虚拟机管理平台，新创建硬盘时，请注意使用以下参数表 3*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^pwy4qpstaxc\|show annotation]]
>
>
>
>
^pwy4qpstaxc


>
>*port TERMINFO=/lib/terminfo/3.5 ==采用 iSCSI 设备作为共享磁盘== 目前针对分库方案，推荐使用 n+1 个 LUN，其中 n 为要分*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^au6lajbxv7i\|show annotation]]
>
>
>
>
^au6lajbxv7i


>
>*4 /dev/data210第 3 章 集群搭建前置操作3.6 ==采用多点可见存储设备作为共享磁盘== 目前，针对分库方案，如果采用非 iSCSI 方式的多点可见存储设*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^pd5s9a9l1ki\|show annotation]]
>
>
>
>
^pd5s9a9l1ki


>
>*中添加如下内容：[core]pager = less –R2) ==配置 crm_dns.conf，路径为/opt/KingbaseHA/crmsh/etc/crm/crm_dsn.conf，其中的 password 为 base64 编码，可通过 echo -n‘123456’| base64 得到编码后的密码== 。# 注意配置需要顶格[Kingbase reourse nam*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^wgnrohe7vzf\|show annotation]]
>
>
>
>
^wgnrohe7vzf


>
>*5看节点是否重启下面是针对 ipmi 环境配置 fencea. ==针对ipmilan 环境配置fence，需要注意IPMI 设备一般为各个节点独占，因此需要为各个节点配置单独的fence资源。== 针对 node1 设置 fencecrm configure p*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^2z8ptqvo0bn\|show annotation]]
>
>
>
>
^2z8ptqvo0bn


>
>*0s \meta failure-timeout=5minb. ==执行如下命令，添加 fence_node1_ipmi 和 fence_node2_ipmi 的 location 约束。== 为了保证 fence_node1_ipmi资源尽量不在节点 no*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^frvdb9o3sam\|show annotation]]
>
>
>
>
^frvdb9o3sam


>
>*fence_node2_ipmi 的 location 约束。 ==为了保证 fence_node1_ipmi资源尽量不在节点 node1，fence_node2_ipmi 资源尽量不在节点 node2，从而减少 fence 自己的概率，又由于资源在各个节点的默认分数为 0，因此需要保证 fence_node1_ipmi 资源在 node2 的分数、fence_node2_ipmi在 node1 的分数均大于 rsc_defaults resource-stickiness 的分数== 。crm configure location fence_no*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^77th3c22mc3\|show annotation]]
>
>
>
>
^77th3c22mc3


>
>*s" \meta failure-timeout=5min3) ==执行如下命令，添加 VIP1 location 约束, 多个节点，最好分数不一样== crm configure location FIP1-on-n*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^puywlk0fcpb\|show annotation]]
>
>
>
>
^puywlk0fcpb


>
>*s" \meta failure-timeout=5min5) ==执行如下命令，添加 FILESYSTEM1 资源，需确保每个节点/sharedata/data1 目录存在，sharedata 属主是root，data1 属主改成数据库用户。使用如下命令查看存储设备/dev/sdc 对应的 uuid== blkid /dev/sdc/dev/sdc: UUID="d8*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^5ev0m9c8nka\|show annotation]]
>
>
>
>
^5ev0m9c8nka


>
>*0" \meta failure-timeout=5min8) ==执行如下命令，添加 PINGD 资源. 目前来说，当网关与集群网络心跳共用同一个网络时，发生网络故障时，将会触发分区故障，而我们总是选择最小的 ID 当主节点，此时如果主节点刚好接口 down 掉，此时集群整个处于不能对外提供服务状态，此时考虑可以设置 failure_action=”reboot”,failure_retries 参数，当集群不能对外提供服务时，自身 reboot，剩余的节点对外提供服务，后面基于 heuristics 测试选主做好之后，可以避免这种情况== 。crm configure primitive PINGD o*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^vpfgtdn0qu\|show annotation]]
>
>
>
>
^vpfgtdn0qu


>
>*0" \meta failure-timeout=5min9) ==执行如下命令，将 PIND 资源，变成 clone 资源== crm configure clone CLONE-PINGD*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^yh0860xryqd\|show annotation]]
>
>
>
>
^yh0860xryqd


>
>*" \meta failure-timeout=5min12) ==执行如下命令，创建 DB1 组资源== crm configure group DB1_GROUP FI*
>[[database/Kingbase/Doc/Cluster manager/RAC/Doc/kingbase RAC 集群--部署手册#^j2fm3f8mtd\|show annotation]]
>
>
>
>
^j2fm3f8mtd
