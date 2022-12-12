---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL jobs manager","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL jobs manager","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-jobs-manager/","dgPassFrontmatter":true}
---


# 1. jobs
```bash

bg将进程搬到后台运行（Background）。
fg将进程搬到前台运行（Foreground）
```


三、常用任务管理命令

```bash
# jobs //查看任务，返回任务编号n和进程号

# bg %n //将编号为n的任务转后台运行

# fg %n //将编号为n的任务转前台运行

# ctrl+z //挂起当前任务

# ctrl+c //结束当前任务
```