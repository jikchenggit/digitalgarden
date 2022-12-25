---
{"author":"aming","email":"jikcheng@163.com","title":"Oracle Linux  Install openvpn","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Oracle Linux  Install openvpn","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Server soft/openvpn","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-server-soft/openvpn/oracle-linux-install-openvpn/","dgPassFrontmatter":true}
---


## Oracle 云主机开放端口

[申请Oracle Cloud永久免费服务+300美元试用额度 - 教程资源|网络资源|首页不显示 - 如有乐享 (ruyo.net)](https://51.ruyo.net/14138.html#13)
# 环境准备
## 软件版本 
| 环境                 | 说明             |
| -------------------- | ---------------- |
| 操作系统             | Oracle Linux 8.6 |
| rsa 密钥生成管理工具 | easy-rsa 3.0.8   |
| OpenVPN              | 2.4.12           | 
  
## 网络环境规划
| 环境               | 说明                                |
| ------------------ | ----------------------------------- |
| VPN客户端地址段    | 10.8.0.0/24                        |
| VPN服务器网卡地址  | 私网10.0.0.104<br>公网140.238.41.64 |
| VPN流量出设备NAT为 | 公网140.238.41.64                   | 


> [!warning]
> 以下操作都需要使用root 用户进行操作， 所以需要切换到root 用户。
> sudo su - root
> 

# 操作系统准备
##  关闭SElinux
```bash
setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/selinux/config
```
## 开启内核转发

```bash
grep -qF "net.ipv4.ip_forward" /etc/sysctl.conf  || echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```
## 关闭防火墙
```bash
systemctl stop firewalld
systemctl disable firewalld
```
# 安装和部署
## 安装第三方源
### 安装并打开epel 源

```bash
yum -y install epel-release
```

```bash
vi /etc/yum.repos.d/oracle-epel-ol8.repo

ol8_developer_EPEL]
name=Oracle Linux $releasever EPEL Packages for Development ($basearch)
baseurl=https://yum$ociregion.$ocidomain/repo/OracleLinux/OL8/developer/EPEL/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```

> [!warning]
> enabled=1

###  安装所需依赖包
```bash
yum install -y openvpn easy-rsa
```

## easy-rsa配置证书密钥
```bash
cp -rf /usr/share/easy-rsa/3.0.8 /etc/openvpn/server/easy-rsa
 cd /etc/openvpn/server/easy-rsa
#复制easy-rsa工具
find / -name vars.example
/usr/share/doc/easy-rsa/vars.example
cp /usr/share/doc/easy-rsa/vars.example  . && mv vars.example vars
#复制vars.example并重命名vars
```
### 配置vars 文件
  配置vars文件，文件也有该内容不过是注释的，可以直接再最后追加如下内容：

```bash
cat << EOF >> /etc/openvpn/server/easy-rsa/vars
set_var EASYRSA_REQ_COUNTRY     "CN"
# 国家
set_var EASYRSA_REQ_PROVINCE    "CQ"
# 省
set_var EASYRSA_REQ_CITY        "ChongQing"
# 城市
set_var EASYRSA_REQ_ORG         "Chen"
# 组织
set_var EASYRSA_REQ_EMAIL       "jikcheng@163.com"
# 邮箱
set_var EASYRSA_REQ_OU          "Chen"
# 拥有者

set_var EASYRSA_KEY_SIZE        2048
# 长度
set_var EASYRSA_ALGO            rsa
# 算法

set_var EASYRSA_CA_EXPIRE      36500
# CA证书过期时间，单位天
set_var EASYRSA_CERT_EXPIRE    36500
# 签发证书的有效期是多少天，单位天
EOF
```

### 生成证书和私钥
```bash
./easyrsa init-pki
Note: using Easy-RSA configuration from: /etc/openvpn/server/easy-rsa/vars

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /etc/openvpn/server/easy-rsa/pk
./easyrsa build-ca nopass
#生成CA证书，需要填写组织名称，随便写。
./easyrsa build-server-full server nopass
./easyrsa gen-dh
openvpn --genkey --secret ta.key
```

### 创建日志存储与用户目录
```bash
mkdir -p /var/log/openvpn/
# 日志存放目录
mkdir -p /etc/openvpn/server/user
# 用户管理目录
chown -R openvpn:openvpn /var/log/openvpn
# 配置权限
```

###  创建用户名密码文件
```bash
echo 'aming Xj3345091@' >> /etc/openvpn/server/user/psw-file
#后续添加用户直接在该文件下添加就可以；
chmod 600 /etc/openvpn/server/user/psw-file
chown openvpn:openvpn /etc/openvpn/server/user/psw-file
```
####
```bash
vi /etc/openvpn/server/user/checkpsw.sh
#!/bin/sh

PASSFILE="/etc/openvpn/server/user/psw-file"
LOG_FILE="/var/log/openvpn/password.log"
TIME_STAMP=`date "+%Y-%m-%d %T"`

if [ ! -r "${PASSFILE}" ]; then
  echo "${TIME_STAMP}: Could not open password file \"${PASSFILE}\" for reading." >>  ${LOG_FILE}
  exit 1
fi
CORRECT_PASSWORD=`awk '!/^;/&&!/^#/&&$1=="'${username}'"{print $2;exit}' ${PASSFILE}`
if [ "${CORRECT_PASSWORD}" = "" ]; then
  echo "${TIME_STAMP}: User does not exist: username=\"${username}\", password=
\"${password}\"." >> ${LOG_FILE}
  exit 1
fi
if [ "${password}" = "${CORRECT_PASSWORD}" ]; then
  echo "${TIME_STAMP}: Successful authentication: username=\"${username}\"." >> ${LOG_FILE}
  exit 0
fi
echo "${TIME_STAMP}: Incorrect password: username=\"${username}\", password=
\"${password}\"." >> ${LOG_FILE}
exit 1
chmod 700 /etc/openvpn/server/user/checkpsw.sh
chown openvpn:openvpn /etc/openvpn/server/user/checkpsw.sh
```
### 创建OpenVPN 服务器配置文件
```bash
cp /usr/share/doc/openvpn/sample/sample-config-files/server.conf /etc/openvpn/server/server.conf
```




# 产生通用profile 文件
```bash
root@oen:/usr/local/openvpn_as/scripts# ./sacli  --user aming  GetUserlogin
```



```bash
sudo find / -name easy-rsa
/usr/share/licenses/easy-rsa
/usr/share/doc/easy-rsa
/usr/share/easy-rsa
```


### 云openvpn 控制台
[AS: oen](https://168.138.210.126/admin/status_overview)


用户名：openvpn
密码： Xj3345091@
[在甲骨文云中启动您自己的免费私有VPN (oracle.com)](https://blogs.oracle.com/developers/post/launching-your-own-free-private-vpn-in-the-oracle-cloud)

[[system/Doc/Linux/Linux Doc/Linux Server soft/openvpn/Oracle 永久免费主机搭建VPN--离线版\|Oracle 永久免费主机搭建VPN--离线版]]

## 配置SSL 
![](https://www.aming.work:8084/images/2022/10/24/20221024172858.png)
