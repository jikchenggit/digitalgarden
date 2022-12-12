---
{"author":"aming","email":"jikcheng@163.com","title":"What is MySQL？","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:05","tags":"What is MySQL？","File Folder with relative path":"database/MySQL/Doc/MySQL Introduction","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-introduction/what-is-my-sql/","dgPassFrontmatter":true}
---


## MySQL信息
 MySQL<sup>TM</sup> 软件提供了一个非常快速,多线程,多用户和健壮的SQL(结构化查询语言) 数据库服务器.MySQL 服务器适用于任务关键型,高负载的生产系统以及嵌入到大规模部署的软件中.

## MySQL 数据库管理系统概述
MySQL 是目前最流行的开源SQL数据库管理系统,是由Oracle 公司开发,发布和支持.
[MySQL 网站](http://www.mysql.com/)提供最新的信息.
- **MySQL 是一个数据库管理系统**
数据库是结构化的数据集合.它可以是任何东西,从一个简单的购物单到一个图片库,或者是公司需要存储大量的信息.要添加,访问和处理存储在计算机的数据的话,需要一个数据库管理系统,如MySQL服务器.由于计算机擅长处理大量数据,所以在IT 架构中数据库是重中之重.
- **MySQL 数据库是一种关系型数据库**
关系型数据库将数据存储到单独的表中,而不是将所有数据放在一个存储空间中.数据库结构会组织程物理文件,以提高速度.具有数据库,表,视图,行和列等对象的逻辑模型提供了灵活的编程环境.可以控制不同的数据之间的关系规则,例如:1 对1 ,1 对多,唯一,必须或者可选.数据库将会前置执行这些规则,因此应用程序将会长时间正确运行.

MySQL 的SQL 是一种结构化查询语言.SQL 是用于访问数据库的最常见的标准化语言可以根据编程环境,直接输入SQL ,或者将SQL 语句嵌入到另一种编程语言中去.

SQL 是有ANSI/ISO SQL 标准定义的,自 1986 年以来,SQL标准一直在发展,已经有了好几个版本.SQL-92 指的是1992 年发布的标准,SQL:1999 是指的是1999 年发布的标准,SQL:2003 是当前的SQL标准.

- **MySQL软件是开源的**
开源意味着任何人都可以使用和修改软件.任何人都可以从互联网上下载MySQL软件并免费使用.还可以研究源代码并修改后满足您的需求.MySQL软件使用GPL (GNU通用公共许可证)可以做什么,不能做什么.如果不想使用GPL ,则可以购买一个商业许可版本.[http://www.mysql.com/company/legal/licensing/](http://www.mysql.com/company/legal/licensing/)

- **MySQL数据库服务器非常快速,可靠,可伸缩,易伸缩**
MySQL 服务器可以运行在台式机或笔记本电脑上,几乎感受不到性能压力.如果将整个汲取用于MySQL,可以调整利用所有可用的内存,CPU ,IO .MySQL 可以进行集群.

- **MySQL服务器在C/S 工作模式**
MySQL 是C/S工作模式 

## 1.2. MySQL 主要特性

本节描述MySQL 数据库软件的一些重要特性.

**内部结构和可移植性**
- 使用C 和C++ 编写的.
- 使用各种不同的编译器进行测试.
- 可以工作在不同的平台上.
- 为了可移植性,在MySQL 5.5 及以上版本中使用CMake.以前的系列使用 GNU Automake,Autoconf和Libtool. 
- 使用Purify(一个商业内存泄漏检测器)以及Valgrind,一个GPL 工具([http://developer.kde.org/~sewardj/)](http://developer.kde.org/~sewardj/))
- 采用独立模块的多层服务器设计.
- 设计完全多线程使用内核线程,以便方柏霓使用多个cpu.
- 提供事务性和非事务性存储引擎.
- 使用b-tree 磁盘表(myisAM)和索引压缩.
- 快速线程内存分配系统.
- 
