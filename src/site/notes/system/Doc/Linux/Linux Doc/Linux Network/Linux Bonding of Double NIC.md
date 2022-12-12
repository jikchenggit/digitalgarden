---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Bonding of Double NIC","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Bonding of Double NIC","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Network","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-network/linux-bonding-of-double-nic/","dgPassFrontmatter":true}
---



```bash
nmcli connection delete eno16777736
nmcli connection delete eno33554984
nmcli connection delete team0
nmcli device
nmcli connection add con-name team0 type team ifname team0 config '("team0":"activebackup"}'
nmcli connection add con-name team0 type team ifname team0 config '{"team0":"activebackup"}'
nmcli 
nmcli device
nmcli connection modify team0 ipv4.addresses 192.168.40.112/24 ipv4.gateway 192.168.40.1 connection.autoconnect yes ipv4.method manual
nmcli connection add type team-slave con-name  team0-slave1 ifname eno16777736 master team0
nmcli connection add type team-slave con-name team0-slave2 ifname eno33554984 master team0
nmcli connection up team0
nmcli connection reload
nmcli connection show
ip a
teamdctl team0 state
````