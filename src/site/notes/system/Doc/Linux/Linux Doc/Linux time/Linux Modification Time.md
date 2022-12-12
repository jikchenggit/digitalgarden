---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Modification Time","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"Linux Modification Time","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux time","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-time/linux-modification-time/","dgPassFrontmatter":true}
---


# Linux 修改硬件时间
使用CentOS，遇到本地时间对不上，直接敲命令：date -s "2017-10-21  15:15:15"是立即生效了，但是重启后，系统时间还是原来的。

修改了其一是没有办法奏效，必须两者都更改。

```bash
1.date                        //查看本地
2.hwclock --show                  //查看硬件的时间
3.如果硬件的时间是对不上，那就对硬件的时间进行修改、
4.hwclock --set --date '2016-01-08  15:15:15'    //设置硬件时间
5.hwclock  --hctosys                //设置系统时间和硬件时间同步
6.clock -w                        //保存时钟
```

7.最后在通过重启，init 6 （reboot）            //重启后，查看系统时间是否真正生效

例1：用指定的格式显示时间。
```bash
$ date '+This date now is =>%x ,time is now =>%X ,thank you !'
This date now is =>11/12/99 ,time is now =>17:53:01,thank you !
```
例2：用预定的格式显示当前的时间。
```bash
# date
Fri Nov 26 15:20:18 CST 1999
```
例3：设置时间为下午14点36分。
```bash
# date -s 14:36:00
Fri Nov 26 14:15:00 CST 1999
```
例4：设置时间为1999年11月28号。
```bash
# date -s 991128
Sun Nov 28 00:00:00 CST 1999
```
实例：设置时间伟2008年8月8号12:00
```bash
# date -s "2008-08-08 12:00:00"
```
修改完后,记得执行`clock -w`，把系统时间写入CMOS


### 设置时间
```bash
timedatectl set-time '2022-01-17 10:56:00'
yum install ntp -y
timedatectl set-ntp yes

```