---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Wake On LAN","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Wake On LAN","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/Wake On LAN","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/wake-on-lan/linux-wake-on-lan/","dgPassFrontmatter":true}
---


# 1. Wake On LAN
我们经常有这样的场景或需求，人在外面，需要将家里的机器或公司的机器开启，进行远程控制操作。
有几种方式可以实现远程开机，一是通过主板的来电自启动，通过智能开关远程开机。还有一种方式就是可以通过一台已经启动的机器通过Wake On LAN去开启唤醒另外一台机器。
现在介绍通过一台已经启动的linux机器通过Wake On LAN去唤醒开期另外一台机器。
* 前置条件：
操作机器与目标机器在同一局域网内
目标机器电源和网线已插好
目标机器网卡和主板均支持远程唤醒并在BIOS里已经设置了网络唤醒(WOL)开机。
目前一般的机器网卡和主板都支持远程唤醒开机，需要在BIOS里设置将网络唤醒开机开启。
开机时进入BIOS，查看CMOS中的“Power Management Setup”，通常里面会有Power On by Onborad Lan，将其设置为“Enable”。
如下图，在电源管理中开启Power On by Onborad Lan。不同的主板BIOS设置不太一样。具体根据自己机器实际情况进行设置。
如何在Linux下通过Wake On LAN远程唤醒，具体操作步骤如下：
* 在本机安装Wake On LAN。可从官方网站下载。
CentOS可以用yum命令安装:
```sh
yum install wol
```
也可以下载wol的rpm包通过rpm安装。
 
* 登录需要远程唤醒开机的目标机器，运行ethtool命令查看网卡是否支持Wake On Lan
```sh
[root@localhost]# ethtool eth0
```
看这两行
```sh
Supports Wake-on: pumbg
Wake-on: d
```
* 若Wake-on为d，表示禁用Wake On LAN，需要启用它。
```sh
[root@localhost]# ethtool -s eth0 wol g
```

网卡会自动恢复禁用状态:
解决方案:

- 编辑/etc/sysconfig/network-scripts/ifcfg-eth0，在里面添加上一行，如下所示：

```vim
ETHTOOL_OPTS="wol g"
```
如果已经是g就说明目标机器的网卡已经支持Wake On LAN。
* 查看目标机器网卡的MAC地址
```sh
[root@localhost]# ifconfig
```
比如获得的MAC地址为00:17:a4:ad:c3:a8
* 关闭目标机器，在主机运行wol命令
```sh
wol   00:e0:8b:68:05:40
```
这时，目标机器这时就会开启了。可以通过ping命令验证机器是否已经启动了。
如果记不住mac地址可以写个简单的shell脚本，直接执行这个脚本就可以了。
```sh
#!/bin/bash
wol 目标mac地址
```