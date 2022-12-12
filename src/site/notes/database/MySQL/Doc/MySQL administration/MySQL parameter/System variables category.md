---
{"author":"aming","email":"jikcheng@163.com","title":"System variables category","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 13:45","tags":"System variables category","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/system-variables-category/","dgPassFrontmatter":true}
---




## 1. 系统变量分类


Understand Server Options, Variables, and the Command Line
理解服务器选项, 系统变量,和命令行参数

服务器选项:顾名思义,使用配置文件中的参数进行配置数据库.
系统变量:分为状态变量和配置变量.

- 命令行参数:
- 使用命令行上进行调用的配置参数.
说明:
    命令行参数个数:4116 个
    只是命令行参数个数:29 个.
命令行参数如下表1:


- 服务器选项参数(option file)
 服务器选项参数: 4069 个 

说明:
服务器选项参数全都被包含再命令行参数里面.

- 系统变量

 系统变量总数:4298
-  命令行 ,服务器选项参数,系统变量
    三者统一重叠数量:3361 个.
 说明:   
     大部分的服务器选项参数和系统变量是一致的.
系统变量和服务器参数一致的只有1 个.  

| Name                            | Cmd-Line | Option File | System Var | Status Var | Var Scope | Dynamic |
| ------------------------------- | -------- | ----------- | ---------- | ---------- | --------- | ------- |
| default\_collation\_for_utf8mb4 |          | Yes         | Yes        |            | Both      | Yes     |

- 状态变量:
  单纯状态变量为:2111个.


说明:
动态参数就一个. 状态变量和 上述变量都不重叠.
      

  
|            Name             | Cmd-Line | Option File | System Var | Status Var | Var Scope | Dynamic |
| --------------------------- | -------- | ----------- | ---------- | ---------- | --------- | ------- |
| original\_commit\_timestamp | 　       | 　          | 　         | Yes        | Session   | Yes     |

      



表1:

| Name                             | Cmd-Line | Option File | System Var | Status Var | Var Scope | Dynamic |
| -------------------------------- | -------- | ----------- | ---------- | ---------- | --------- | ------- |
| [defaults-extra-file]            | Yes      | 　          | 　         | 　         | 　        | 　      |
| [defaults-file]                  | Yes      | 　          | 　         | 　         | 　        | 　      |
| [defaults-group-suffix]          | Yes      | 　          | 　         | 　         | 　        | 　      |
| [install]                        | Yes      | 　          | 　         | 　         | 　        | 　      |
| [install-manual]                 | Yes      | 　          | 　         | 　         | 　        | 　      |
| [local-service]                  | Yes      | 　          | 　         | 　         | 　        | 　      |
| ndb-transid-mysql-connection-map | Yes      | 　          | 　         | 　         | 　        | 　      |
| [no-defaults](.                  | Yes      | 　          | 　         | 　         | 　        | 　      |
| [print-defaults]                 | Yes      | 　          | 　         | 　         | 　        | 　      |
| [remove]                         | Yes      | 　          | 　         | 　         | 　        | 　      |
| [slave-pending-jobs-size-max]    | Yes      | 　          | 　         | 　         | 　        | 　      |

附:
