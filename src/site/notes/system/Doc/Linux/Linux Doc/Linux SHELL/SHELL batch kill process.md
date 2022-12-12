---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL batch kill process","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"SHELL batch kill process","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-batch-kill-process/","dgPassFrontmatter":true}
---


[Linux]使用awk批量杀进程的命令
碰到需要杀掉某一类进程的时候，如何批量杀掉这些进程，使用awk命令是很好的选择。

```sh
ps -ef|grep aaa|grep -v grep|awk  '{print "kill -9 " $2}' |sh
```

* 测试找出进程关键字是否准确
```sh
. ps -ef|grep aaa|grep -v grep   
```

> 这是大家很熟悉的命令，这里就不再多说，就是从当前系统运行的进程的进程名中包含aaa关键字的进程。
后面部分就是awk命令了，一般awk命令的格式为：awk ' pattern {action} '
print是打印，kill -9 是强制停止进程的命令， $2就是前面有ps -ef命令得出的结果的第二列上显示的内容。

例子：

```sh
#ps -ef|grep boco|grep -v grep
    root  9884  9883  0 17:10:01 ?         0:00 sendmail -oem -oi -froot boco
    root  9883  9880  0 17:10:01 ?         0:00 /usr/bin/mail boco
    boco 11112     1  0  Dec 24  ?         0:00 ./boco_appmaster -d /home/boco/agent
    boco 11126 11125 61  Dec 24  ?        52:59 ./boco_hostagent -i socket -l
    boco 11125 11112 37  Dec 24  ?        43:25 ./boco_appmaster -d /home/boco/agent
    boco  9811 11113  0 17:09:31 ?         0:00 sleep 60
    boco 11113     1  0  Dec 24  ?         0:23 /bin/sh ./boco_monitor.sh
#ps -ef|grep boco|grep -v grep|awk '{print "kill -9 "$2}'
kill -9 9884
kill -9 9883
kill -9 11112
kill -9 11126
kill -9 11125
kill -9 9811
kill -9 11113
```

 

我们可以看出，ps -ef|grep boco|grep -v grep列出了当前主机中运行的进程中包含boco关键字的进程

而ps -ef|grep boco|grep -v grep|awk '{print "kill -9 "$2}'则列出了要kill掉这些进程的命令，并将之打印在了屏幕上

在
ps -ef|grep boco|grep -v grep|awk '{print "kill -9 "$2}'后面加上|sh后，则执行这些命令，进而杀掉了这些进程。

* 找出进程直接杀：
```sh
ps -aux|grep "service_2.php"|awk '{print "kill -9 "$2}'|sh
```