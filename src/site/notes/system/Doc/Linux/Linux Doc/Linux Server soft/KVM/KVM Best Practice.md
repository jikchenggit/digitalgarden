---
{"author":"aming","email":"jikcheng@163.com","title":"KVM Best Practice","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"KVM Best Practice","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/KVM","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/kvm/kvm-best-practice/","dgPassFrontmatter":true}
---


# 1. 安装步骤
# 2. KVM配置及维护
## 2.1. kvm使用场景

1.公司测试环境/开发环境
测试开发环境可以使用配置低点的物理机就可以
2.公司生产环境
一般小公司没有私有云或容器团队，运维人员可能就1-2个，然后公司也不舍得花钱买商业化的私有云。
那么在这种情况下搞一台或多台高配的物理机里面装多个虚拟机，可以设置基础的虚拟机模板或根据不同业务设置不同的虚拟机模板，完成初步的环境标准，便于以后自动化运维。          
## 2.2. KVM介绍   
KVM（Kernel-based Virtual Machine）是一个linux的内核模块，现在已经是内核自带默认编译，不需要单独安装。主要负责控制cpu和内存跟内核的交互调度等工作，工作在内核态。
KVM需要CPU中虚拟化功能的支持，只可在具有虚拟化支持的CPU上运行，即具有VT功能的Intel CPU和具有AMD-V功能的AMD CPU。
* KVM 虚拟化特性： 
• 嵌入到Linux正式Kernel（提高兼容性）
• 代码级资源调用（提高性能）
• 虚拟机就是一个进程（内存易于管理）
• 直接支持NUMA技术（提高扩展性）
• ---------- RedHat收购KVM -----------
• 保持开源发展模式 更好的商业支持及服务保障
* 虚拟机镜像：
• 全镜像模式-RAW
• 稀疏模式-QCOW2
## 2.3. KVM 管理工具：
## 2.4. Libvirt介绍
libvirt 提供一种虚拟机监控程序不可知的 API 来安全管理运行于主机上的客户操作系统。libvirt 本身不是一种工具， 它是一种可以建立工具来管理客户操作系统的 API。libvirt 本身构建于一种抽象的概念之上。它为受支持的虚拟机监控程序实现的常用功能提供通用的 API。libvirt 起初是专门为 Xen 设计的一种管理 API，后来被扩展为可支持多个虚拟机监控程序。

## 2.5. QEMU介绍
有两种主要运作模式：
* User mode模拟模式，亦即是用户模式。QEMU能启动那些为不
同中央处理器编译的Linux程序。而Wine及Dosemu是其主要目标。
* System mode模拟模式，亦即是系统模式。QEMU能模拟整个电脑系统，
包括中央处理器及其他周边设备。它使得为跨平台编写的程序进行测试及除错工作变得容易。
其亦能用来在一部主机上虚拟数部不同虚拟电脑。
配置步骤

# 3. CentOS 7.2 系统最小化安装
## 3.1. 关闭Selinux，Firewalld，Networkmanager,配置dns服务器ip
```sh
[root@192-168-x-x img]# systemctl disable NetworkManager
[root@192-168-x-x img]# systemctl disable firewalld
[root@192-168-x-x img]# grep ^SELINUX /etc/selinux/config 
SELINUX=disabled
[root@192-168-x-x network-scripts]# more /etc/resolv.conf 
nameserver 114.114.114.114
nameserver 8.8.8.8
```

## 3.2. 配置阿里云yum源
rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
## 3.3. 安装系统常用软件包
```sh
yum install net-tools vim screen mtr  nc nmap tree lrzsz open-ssl-devel gcc glibc gcc-c* make zip dos2unix  systat mysql lsof tcpdump ntpdate -y
```
## 3.4. 物理机确认bios确认是否开启虚拟机技术
虚拟化 Inter VT-x/EPT(内存虚拟化)或 AMD-V/RVI(V) 开启
```sh
grep -E '(vmx|svm)' /proc/cpuinfo
```
## 3.5. 安装kvm相关的软件包
kvm是内核虚拟机，需要加载内核模块,libvirtd是用于管理kvm虚拟机
```sh
yum install -y qemu-kvm  qemu-kvm-tools  libvirt
```
## 3.6. 启动libvirtd
```sh
systemctl  start libvirtd
systemctl  enable libvirtd
```
## 3.7. 安装文件存放的目录，qemu目录是存放虚拟机xml文件
        xml是libvirt 自动生成，xml里面的virtio是半虚拟化渠道，hvm是硬件虚拟机，还有一些其他的虚拟机配置信息
```sh
[root@192-168-x-x libvirt]# ls
libvirt-admin.conf  libvirt.conf  libvirtd.conf  lxc.conf  nwfilter  qemu  qemu.conf  qemu-lockd.conf  storage  virtlockd.conf  virtlogd.conf
[root@192-168-x-x libvirt]# pwd
/etc/libvirt
```
>  启动libvirtd后，会自动创建virbr0 桥接网卡 IP地址永远都是192.168.122.1
可以用ifconfig命令查看

```#ifconfig
......
virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:5c:f1:a5  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## 3.8. 查看dnsmasq进程，dns和dhcp功能 帮助分配虚拟机，dnsmasq无需配置启动就行

```sh
[xiewenming@192-168-x-x ~]$ ps aux |grep dnsmasq
nobody     2992  0.0  0.0  15544   152 ?        S    Mar14   0:36 /sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
root       2993  0.0  0.0  15516     4 ?        S    Mar14   0:00 /sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
xiewenm+  35304  0.0  0.0 112652   960 pts/3    S+   15:38   0:00 grep --color=auto dnsmasq
```

   NOTE :安装virt-install，virt-edit，virt-manager，tigervnc-server和tigervnc相关的包，分别用于 安装，编辑，管理，vnc服务来管理维护虚拟机
## 3.9. libvirt 管理kvm虚拟机
可以提供接口 virsh等 底层都是libvirt libvirtd服务停止 kvm正常，只是管理不了，virsh是用C写的
```sh
yum install -y virt-install libguestfs-tools-c  virt-manager tigervnc-server tigervnc
```
## 3.10. 配置网卡桥接方法一
使用命令重启服务器或重启网络后配置丢失 （一般用于调试用）
```sh
brctl addbr br0
brctl addif br0 eth0 #（执行这一步会断网，可以把所有步骤写在一个脚本里）
ip addr del dev eth0 192.168.x.x/24 
ifconfig br1 192.168.x.x/24 up
route add default gw 192.168.x.1
```
## 3.11. 配置网卡桥接方法二
 桥接信息写到配置文件里面，下面以桥架网卡1为例

```sh
[root@192-168-x-x network-scripts]# more ifcfg-br1 
TYPE=Bridge
BOOTPROTO=none
DEVICE=br1
ONBOOT=yes
IPADDR0=192.168.x.x
PREFIX0=24
GATEWAY0=192.168.x.1
[root@192-168-x-x network-scripts]# more ifcfg-eth1 
DEVICE=eth1
TYPE=Ethernet
ONBOOT=yes
BRIDGE=br1
[root@192-168-x-x network-scripts]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.x.10    0.0.0.0         UG    0      0        0 br1
169.254.0.0     0.0.0.0         255.255.0.0     U     1004   0        0 br0
169.254.0.0     0.0.0.0         255.255.0.0     U     1005   0        0 br1
192.168.x.0     0.0.0.0         255.255.255.0   U     0      0        0 br1
192.168.122.0   0.0.0.0         255.255.255.0   U     0      0        0 virbr0
```

## 3.12. 安装图形桌面方便vnc管理
```sh
yum groupinstall "X Window System"
yum install gnome-classic-session gnome-terminal nautilus-open-terminal control-center liberation-mono-fonts
unlink /etc/systemd/system/default.target
ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target
```
## 3.13. 配置vncserver及密码默认端口是从5901开始

```sh
[root@192-168-x-x network-scripts]# vncpasswd 
Password:     #设置密码
[root@192-168-x-x network-scripts]# vncserver 
New '192-168-x-x.xxx.com:1 (root)' desktop is 192-168-x-x.xxx.com:1
Starting applications specified in /root/.vnc/xstartup
Log file is /root/.vnc/192-168-x-x.xxx.com:1.log
[root@192-168-x-x network-scripts]# netstat -an|grep 0.0.0.0:59
tcp        0      0 0.0.0.0:5901            0.0.0.0:*               LISTEN  
```

## 3.14. vnc客户端连接管理
 

## 3.15. 登陆以后，打开控制台使用virt-manager打开虚拟机管理界面

## 3.16. 进行虚拟机维护
可以关机，重启，删除，增加删除配置，安装系统等等 ，也可以支持安装Windows系统虚拟机

# 4. 常用的维护命令

* 可以使用命令安装虚拟机
virt-install创建虚拟机，不熟悉可以用virt-install --help查看帮助命令
```sh
virt-intall --virt-type kvm --name CentOS-7-x86_64 --ram 2048 --cdrom=/Data/CentOS-7-x86_64-DVD-1503-01.iso \
--disk path=/Data/CentOS-7-x86-_64.raw --network network=default --graphics vnc,listen=0.0.0.0 --noautoconsole
```
用VNC客户端链接（60s内） 默认从5900开始，客户端安装截图略....我一般都通过vnc然后再用virt-manager进行管理虚拟机



| 命令                                                                    | 说明                                                                          |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| brctl show                                                              | 查看桥接网卡信息                                                             |
| virsh edit vname.xml                                                    | 修改xml文件                                                                  |
| virsh shutdown vname                                                    | 关闭虚拟机                                                                   |
| virsh start vname                                                       | 启动虚拟机                                                                   |
| virsh list --all                                                        | 列出全部虚拟机                                                               |
| virsh dumpxml vname > vname.bak.xml                                     | 备份                                                                         |
| qemu-img convert -f raw -O qcow2 vname test.qcow2                       | 镜像文件格式转换                                                             |
| qemu-img create -f raw node-192.168.3.30-centos-7.2.disk 50G            | 创建磁盘镜像命令                                                             |
| virsh undefine node1.xml                                                | 删除虚拟机域，生产环境不用此命令，非常危险）                                |
| virt-clone -o centos7_mini -n centos7_mini15 --auto-clone               | 克隆mini，新克隆的为mini15                                                   |
| -o                                                                      | 原始机名字，必须为关闭或暂停状态                                             |
| -n                                                                      | 新客户机的名称                                                               |
| --auto-clone                                                            | 从原始客户机配置中自动生成克隆名称和存储路径                                 |
| --replace                                                               | 不检查命名冲突，覆盖任何使用相同名称的客户机                                 |
| -f                                                                      | 可以指定克隆后的主机镜像放在指定目录下                                       |
| virsh autostart xxx                                                     | 让子机随宿主机开机自动启动                                                   |
| virsh autostart --disable xxx                                           | 解除自动启动                                                                 |
| virt-install                                                            | 建立kvm虚拟机                                                                |
| virsh list                                                              | 查看正在运行的KVM虚拟机                                                      |
| virsh start name                                                        | 启动KVM虚拟机                                                                |
| virsh shutdown name                                                     | 正常关闭KVM虚拟机                                                            |
| virsh destroy name                                                     | 强制关闭KVM虚拟机(类似于直接断电)                                            |
| virsh suspend name                                                      | 挂起KVM虚拟机                                                                |
| virsh resume name                                                       | 恢复挂起的KVM虚拟机                                                          |
| virsh dumpxml name                                                      | 查看KVM虚拟机配置文件，可以把输出的内容定义到xml里，用来克隆迁移用。         |
| virsh edit name                                                         | 编辑KVM虚拟机的xml配置文件                                                   |
| virsh define /etc/libvirt/qemu/name.xml                                 | 定义注册虚拟机，需要先查看xml文件对应的镜像，img等路径是否存在或修改指定路径 |
| virsh undefine name                                                     | 彻底删除KVM虚拟机,不可逆,如果想找回来,需要备份/etc/libvirt/qemu的xml文件     |
| virsh attach-disk centos7_mini15 /home/disk_data/centos7_mini15.img vdb | 分离虚拟机硬盘                                                               |
| virsh detach-disk centos7_mini15 /home/disk_data/centos7_mini15.img vdb | 为虚拟机添加硬盘                                                             |

遇到的一些问题  

1.之前用CentOS7.3的时候windows虚拟机安装的时候黑屏，一直没有找到解决办法，最后没有办法就降CentOS7.2，所有建议生产环境不用使用CentOS7.3部分服务有兼容的不太好
2.磁盘满了部分虚拟机进程被系统kill掉
3.没有可用内存时，也会出现部分虚拟机被系统kill掉，严重的情况下物理服务器可能会自动重启（所有在使用的过程中资源尽量不要超配）
4.有时可能会出现桥接的网卡和时间配置的网卡不是同一个 可以用ifconfig -a查看

来自 <https://www.cnblogs.com/xiewenming/p/7527476.html> 
