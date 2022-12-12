---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL error Log","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"MySQL error Log","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-error-log/","dgPassFrontmatter":true}
---




## 数据库错误日志语言
**概述** 
默认情况下MySQL 生成错误消息为English 语言.当然可以设置为,捷克语、丹麦语、荷兰语、爱沙尼亚语、法语、德语、希腊语、匈牙利语、意大利语、日语、韩语、挪威语、挪威-纽约语、波兰语、葡萄牙语、罗马尼亚语、俄语、斯洛伐克语、西班牙语或瑞典语.

MySQL 使用以下语言规则搜索错误消息:
```sql
SET lc_messages = 'en_US';
```

## MySQL 的error log 组件
想要设置error log 当然对于MySQL error log 的组件要有一定的了解.

在 MySQL 8.0 ,错误日志的架构在," MySQL 的组件" 有说明.错误日志的子系统包含有事件过滤器和写日志功能.使用这些组件可以实现输出所需的日志资源.

对于错误日志选择什么样的组件. 对于特定日志和JSON 日志的写入,请参考 "错误日志记录" 和" JSON 日志格式" ,对于附加的组件,,请参见 "错误日志 组件" 章节.

对于基于组件的错误日志提供了以下功能:

 - 可以用筛选器筛选错误日志,可以输出指定信息.

  - 日志可以用sink(写入)组件进行输出,可以启用多个写入组件,将错误日志输出到多个目的地.

 - 过滤器和编写器组件组合起来就可以设定特定的错误格式.

 - 可以配置写组件到系统日志.

 - 可以配置写组件为JSON 格式.
 
### 配置规则和范例

#### 参数次序说明
通过系统变量来配置 日志组件的功能是否开启 和配置 规则.

log_errot_services 系统变量控制要启用哪些日志组件,所以log_errot_services 日志顺序很重要.执行顺序是从左往右依次执行. 

- 对于默认值为:
```sql
mysql> SELECT @@global.log_error_services;
+----------------------------------------+
| @@global.log_error_services            |
+----------------------------------------+
| log_filter_internal; log_sink_internal |
+----------------------------------------+
1 row in set (0.00 sec)
```
说明:以上的意思是首先通过log_filter_internal 功能组件.然后通过日志构建组件,log_sink_internal. 格式化成特定的格式并写入相关输出目的地,如文件或系统日志.

:::alert-warning
如果 log_error_services 参数没有指定写组件,将没有日志输出.
:::

log_filter_internal 和log_sink_internal 组合实现了默认的错误日志过滤和输出.这些组件的动作受到其他服务器选项和系统变量的影响:

输出目的地由log-error 决定(在windows 上 ,由pid-file 和console决定).这些参数决定是否把错误写入控制台或者文件,如果写入文件,则设置错误日志的文件名.

####  过滤规则

1. log_error_verbosity 变量影响到log_filter_internal 的过滤规则.
####  设置变量值和加载组件

如果想要更改错误日志.只需要根据规则修改log_error_services 值.增加或者减少日志组件要根据以下几个规则.

- 要开启日志组件,首先使用install component 加载此功能.然后在log_error_services 中列出需要的组件.
  
 设置log_error_services 值的时候,组件必须是已知的组件名.否则,如果服务器在启动之后如果服务器不能识别这个组件,将会导致报错,并且设置log_error_services 参数为默认值.

- 如果想要禁用日志组件,请删除log_error_services 的所有值.如果这个日志组件是可加载的,使用UNINSTALL COMMPONET 语句卸载这个插件.

:::alert-warning
如果log_error_services 中的值未删除,尝试删除组件将会导致错误.
:::

  
##### 更改参数写入驱动器
如果想使用系统日志写入器(log_sink_syseventlog) 而不是默认的写入器(log_sink_internal),首先先加载写入器,然后再修改log_error_services 的值. 设置

```sql
mysql> INSTALL COMPONENT 'file://component_log_sink_syseventlog';
Query OK, 0 rows affected (8.43 sec)

mysql> SET GLOBAL log_error_services = 'log_filter_internal; log_sink_syseventlog';
Query OK, 0 rows affected (0.02 sec)

```

:::alert-warning
这个 URN 使用了 INSTALL COMPONENT 组件使用 前缀  `file://component_. `
例如:对于log_sink_sysenventlog 组件,正确的URN 是  `file://component_log_sink_syseventlog.    `
:::

##### 如果想让日志输出到多个目的地,则需要增加log_error_services 参数像下面一样.

```sql
SET GLOBAL log_error_services = 'log_filter_internal; log_sink_internal; log_sink_syseventlog';
```
#### 还原默认值

```sql
SET GLOBAL log_error_services = 'log_filter_internal; log_sink_internal';
UNINSTALL COMPONENT 'file://component_log_sink_syseventlog';
```
#### 配置开启启动

1. 插件如果是可以加载的请运行以下命令进行加载.例如配置JSON 日志写入器.

```sql

INSTALL COMPONENT 'file://component_log_sink_syseventlog';
INSTALL COMPONENT 'file://component_log_sink_json';
```
2. 这个组件便会注册到 mysql.component 表中.

```sql
select * from mysql.component;
```
3. 设置开机参数.
可以使用两种方式:
    case1: 设置my.cnf 
```
[mysqld]
     log_error_services='log_filter_internal; log_sink_internal; log_sink_json'
```
     case2:命令行操作 ,并在my.cnf 设置值.
```sql
    SET PERSIST log_error_services = 'log_filter_internal; log_sink_internal; log_sink_json';
```





### 参数顺序

log_filter_internal; log_sink_1; log_sink_2
在这个例子中日志传输到内置过滤器,然后传递到第一个写入器,然后再传入第二个写入器.


log_sink_1; log_filter_internal; log_sink_2
这个例子是个错误示范:首先写入器1 接受全部日志,然后再传入过滤器,再传入写入器2.

## 数据库错误日志设置

**概述**
  

这篇文章主要讨论怎样配置MySQL 的诊断日志信息.还有对于设置错误信息的字符集和语言设置.

这些错误日志包含了MySQL 启动和关闭的次数.也包含了错误,警告,和注释的相关诊断信息.
MySQL 在运行时,如果你的MySQL 中的表需要自动检查或者修复.这些信息都会写入到error log 里面.

在某些的操作系统,错误日志还包含MySQL 非正常的退出的堆栈信息. 这些trace 可以用于调试和debugg MySQL.

如果启动MySQL 时,如果MySQL 没有正常关闭,则也会往error log里面写入信息.

下面的章节会详细介绍错误日志的配置和描述.

## 配置日志输出目的地
### windows 的默认日志输出.
再windows 服务器上 --log-error ,--pid-file 和--console 选项决定了错误日志为error log 的目的地,是console 还是file ,如果是file 则文件名是什么.

1. 如果 --console 选项启用,默认输出就是console.如果--log-error 同时被指定,则--log-error 不会生效. 

2. 如果--log-error  没有指定,或者给了一个没有名字的文件.默认的目的地为 host_name.err 再数据目录下.除非指定了--pid-file 选项.则文件名为数据目录下 后缀文件.err 的PID 文件名.

3. 如果--log-error 给了一个文件名.如果没有给后缀名则添加.err 后缀名.文件默认再DATA 目录下.除非使用了绝对路径进行指定.

:::alert-warning
如果默认的错误日志为console ,log_error 变量则为stderr ,否则 将会是个文件.
:::
### Linux 的默认日志输出.
再Unix 和 类Unix 系统上,MySQL 将使用--log-error 选项决定日志的输出目的地是console 或者是个文件.

1. 如果 --log-error 没有设置,默认输出则为console.

2. 如果--log-error 设置了没有文件名的一个文件.默认值为host_name.err 在data 目录下.

3. 如果--log-error 给了一个文件名.如果没有给后缀名则添加.err 后缀名.文件默认再DATA 目录下.除非使用了绝对路径进行指定.

4. 如果--log-error 设置于[mysqld],[server],或者[mysqld_safe] 章节.则MySQL j将会应用这一选项.

:::alert-warning

通常使用Yum 或者 APT 包安装的MySQL ,error log 在/var/log目录下面.log-error 设置为 log-error=/var/log/mysqld.log ,如果把mysqld.log 去掉,变为log-error=/var/log/ 则会导致MySQL 将会使用hostname.err 文件来记录错误日志.
:::

:::alert-warning
如果默认的错误日志为console ,log_error 变量则为stderr ,否则 将会是个文件.
:::
## 配置日志输出到系统日志中
MySQL 允许把error log 配置到system log 中去.

以下主要是配置内置过滤器和log_filter_iinternal 和 log_sink_sysenventlog 来配置错误日志.


:::alert-warning

在 MySQL 8.0 中,必须显式配置系统错误日志.和MySQL 5.7 以前的版本有所不同,MySQL5.7 之前默认启用了对系统日志的记录功能,并且不需要加载组件.
:::

1. 安装组件,启用组件

```sql

INSTALL COMPONENT 'file://component_log_sink_syseventlog';
SET GLOBAL log_error_services = 'log_filter_internal; log_sink_syseventlog';
------ 如果想设置永久有效请使用以下命令.
 SET PERSIST log_error_services = 'log_filter_internal; log_sink_syseventlog';
```


Note
:::alert-warning

对于系统日志进行记录需要额外的系统配置请参见系统日志相关文件.
:::

### windows 的日志格式信息.

错误,警告,注意条目将会写入系统日志. 存储引擎的语句将不会被记录.

日志条目有MySQL 标志.

### Unix and Linux 

log_syslog_facility:往系统日志中写入的工具.默认为daemon.

log_syslog_include_pid:系统日志中是否包含进程ID 信息.

log_syslog_tag:向系统日志中记录MySQL error log 是否添加mysqld 标记,

设置日志在MySQL 中变量名为:

```sql

mysql> show variables like '%syseventlog%';
+-------------------------+---------+
| Variable_name           | Value   |
+-------------------------+---------+
| syseventlog.facility    | daemon  |
| syseventlog.include_pid | ON      |
| syseventlog.tag         | [MySQL] |
+-------------------------+---------+
3 rows in set (0.02 sec)
```


MySQL 使用system 标签来表示非错误的情况的重要信息,例如启动关闭和设置重要的更改.包括window 上的事件日志,unix 和Linux 系统的syslog,系统消息被分配.当MySQL log_error_varbosity 设置将这些信息排除,这些信息也会打印到日志中.

如果系统有其他配置将会丢弃information 级别的消息,或者把这些信息重定向到不同的位置.则此配置不会阻止这种行为.除非进行自定义设置.
测试结果:

```
tail -f /var/log/messages
Apr 28 19:15:35 dbserver systemd: Stopped MySQL Server.
Apr 28 19:15:35 dbserver systemd: Starting MySQL Server...
Apr 28 19:15:42 dbserver qemu-ga: info: guest-ping called
Apr 28 19:15:43 dbserver mysqld-[MySQL][13640]: /usr/sbin/mysqld (mysqld 8.0.15) starting as process 13640
Apr 28 19:15:43 dbserver mysqld-[MySQL][13640]: CA certificate ca.pem is self signed.
Apr 28 19:15:43 dbserver systemd: Started MySQL Server.
Apr 28 19:15:43 dbserver mysqld-[MySQL][13640]: /usr/sbin/mysqld: ready for connections. Version: '8.0.15'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MySQL Community Server - GPL.
Apr 28 19:15:43 dbserver mysqld-[MySQL][13640]: X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock' bind-address: '::' port: 33060
```
##  配置json 日志格式

### 开启json 写入器,并使用.
先卸载不使用的插件:
```sql

mysql> select * from mysql.component;
+--------------+--------------------+---------------------------------------+
| component_id | component_group_id | component_urn                         |
+--------------+--------------------+---------------------------------------+
|            1 |                  1 | file://component_validate_password    |
|            3 |                  3 | file://component_log_sink_syseventlog |
|            5 |                  4 | file://component_log_sink_json        |
+--------------+--------------------+---------------------------------------+
3 rows in set (0.00 sec)
```
```sql
SET PERSIST log_error_services  = ''
UNINSTALL COMPONENT 'file://component_log_sink_syseventlog';
```

```sql
UNINSTALL COMPONENT 'file://component_log_sink_json';
INSTALL COMPONENT 'file://component_log_sink_json';

SET GLOBAL log_error_services = 'log_filter_internal; log_sink_json';
----设置永久变量
SET PERSIST log_error_services = 'log_filter_internal; log_sink_json';
```



可以设置以下规则,以下规则则使用json 写入器先不过滤任何日志进行全写入,然后通过内部过滤器进行json 日志写入目的地.

```sql
SET GLOBAL log_error_services = 'log_sink_json; log_filter_internal; log_sink_json';
```

JSON 日志写入器根据默认的log_error 的文件名为基准.加上一个.nn.json 后缀名.例如:
log_error 的值为file_name ,则log_error_services 值为log_sink_json 的写入器,则生成的日志文件为file_name.00.json,file_name.01.json 等等.

如果log_error 是srderr ,则json 写入器将写入控制台,如果log_json_writer 多次调用.那么都写入控制台.

::: alert-danger
json 日志在MySQL 8.0.15  是无法被设置的会出现错误:
`
ERROR 1210 (HY000): Incorrect arguments to SET.
`
据查是个bug.
:::

## 错误日志过滤器

错误日志配置通常包括一个日志过滤器,和一个或多个日志写入器组件.
对于过滤器提供了组件:

log_filter_internal: 这是过滤器组件是默认启用,是由log_error_verbosity 变量设置事件的优先级进行过滤,
dragnet.log_error_filte: 提供基于用户提供的规则的错误日志过滤规则.

###  log_filter_internal :基于优先级的错误日志过滤规则.
错误日志的冗余控制是基于优先级的日志过滤的简单形式.使用log_filter_internal 日志过滤器组件实现.设置log_error_verbosity 将会影响log_filter_internal 的过滤规则.默认情况下是启用的.但如果禁用这一选项,更改log_error_verbosity 不会有任何影响.

log_error_verbosity 设置1(仅限错误),2(错误和警告),3(错误,警告和注释)

log_error_verbosity 设置为2 或者更高级别,服务器将会记录关于基于不安全的语句消息.
log_error_verbosity 设置为3 ,服务器将记录尝试终止的连接和拒绝访问的错误.

如果使用replication ,建议将log_error_verbosity 设置为2 或者更大,以便获取更多信息用于诊断.

如果 replication 的slave 端设置 log_error_verbosity 设置2 或者更高,slave 将打印相关的状态信息.例如: 二进制日志和复制日志的位置.当切换到另一个复制日志上,当重新连接时.

不论log_error_verbosity 的设置如何,有些重要的系统消息都会打印到错误日志中.包括启动和关闭消息,设置的重要更改.

MySQL 的错误日志中,系统消息被标记为"system".其他日志写入器可能遵循或不遵循相同的预定.

### log_filter_dragnet:基于规则的日志过滤器
log_filter_dragnet 日志过滤器支持用户自定义日志过滤.如果要配置规则请设置: log_error_filter_rules 系统变量.

首先安装log_filter_dragnet 过滤器,然后设置log_error_services

```sql

INSTALL COMPONENT 'file://component_log_filter_dragnet';
SET GLOBAL log_error_services = 'log_filter_dragnet; log_sink_internal';
---启动生效
 SET PERSIST log_error_services = 'log_filter_dragnet; log_sink_internal';
```
设置规则:
log_error_filter_rules 系统变量规则由零个或多个规则组成.每个规则是if 语句,以(.) 接数.如果变量值为空(无规则),则不进行过滤.


Example1:规则删除信息事件,其他事件将删除source_line 字段.默认值也为这个.

```sql
SET GLOBAL dragnet.log_error_filter_rules =
  'IF prio>=INFORMATION THEN drop. IF EXISTS source_line THEN unset source_line.';
  -- 永久设置
 SET PERSIST dragnet.log_error_fileter_rules =
  'IF prio>=INFORMATION THEN drop. IF EXISTS source_line THEN unset source_line.';
```
::: alert-info

效果等同于使用当log_error_verbosity =2 时的log_sink_internal 的过滤.
:::
Example 2: 此规则将信息事件限制为每秒60秒不超过一个:

```sql

SET GLOBAL dragnet.log_error_filter_rules = 'IF prio>=INFORMATION THEN throttle 1/60.';
-- 永久设置
SET PERSIST dragnet.log_error_filter_rules = 'IF prio>=INFORMATION THEN throttle 1/60.';
```
- 删除自定义过滤语言
```sql
SET GLOBAL log_error_services = 'log_filter_internal; log_sink_internal';
```
::: alert-info

建议使用永久设置.防止重启后不生效.
:::

 - 卸载组件

```sql
UNINSTALL COMPONENT 'file://component_log_filter_dragnet';
```
#### 过滤语言详细设置
详细介绍log_filter_dragnet 的详细设置

-  规则语言

- 规则行为

- 规则领域

##### 规则语言

```
## 语言框架
rule:
IF condition THEN action
    [ELSEIF condition THEN action] ...
    [ELSE action]
.
## 有多个判断语句时使用ELSEIF 条件和AND  运算符

##  条件 
condition: {
    field comparator value
  | [NOT] EXISTS field
  | condition {AND | OR}  condition
}

## 动作
action: {
    drop
  | throttle {count | count / window_size}
  | set field [:= | =] value
  | unset [field]
}

## 领域
field: {
    core_field
  | optional_field
  | user_defined_field
}

### 核心领域参数
core_field: {
    time
  | msg
  | prio
  | label
  | err_code
  | err_symbol
  | SQL_state
  | subsystem
}

### 可选领域参数
optional_field: {
    OS_errno
  | OS_errmsg
  | user
  | host
  | thread
  | query_id
  | source_file
  | source_line
  | function
}

### 自定义领域字符集
user_defined_field:
    sequence of characters in [a-zA-Z0-9_] class

## 表达式参数
comparator: {== | != | <> | >= | => | <= | =< | < | >}

## 值参数
value: {
    string_literal
  | integer_literal
  | float_literal
  | error_symbol
  | severity
}

### count 参数
count: integer_literal
window_size: integer_literal

###字符串参数
string_literal:
    sequence of characters quoted as '...' or "..."

### 整数值参数
integer_literal:
    sequence of characters in [0-9] class

### 浮点值参数
float_literal:
    integer_literal[.integer_literal]

### 特殊符号参数
error_symbol:
    valid MySQL error symbol such as ER_ACCESS_DENIED_ERROR or ER_STARTUP

### 严重级别
severity: {
    ERROR
  | WARNING
  | INFORMATION
}
```

:::alert-warning
简单的条件将字段与值 或者测试字段的存在性进行比较,若要构造更负载的条件,请使用AND 和 OR 元素安抚.这两个运算符由相同的优先级. 从左到右求值.
:::

::: alert-info

要转义字符串的字符,请在前面加上(\)
:::




 - 事件严重值1,2,3 可以指定为错误,警告,信息.符号与prio 字段进行比较时才能识别.

IF prio == INFORMATION THEN ...
IF prio == 3 THEN ...

 - 错误代码也可以使用数字形式指定. ER_STARTUP 是错误1408 的符号名,

IF err_code == ER_STARTUP THEN ...
IF err_code == 1408 THEN ...
如果符号名称被加上了引号("),也就变成了普通的字符串,这样这种字符串是没有特殊意义的,log_filter_dragnet 不会解释成特殊的数值.

#### 规则行为
支持的操作为以下操作

1. drop: 删除当前日志事件(不记录).
2. threottl: 对于匹配特定条件的事件应用速率限制减少日志的冗余.参数为count 或 count/window_size 的形式指定一个速率,count 值表示每个时间段允许记录的事件数量.window_size 值单位为秒,如果省略则为60秒.

E1: 这条规则将插件关闭消息进行节流,每60 秒5条.

```sql
IF err_code == ER_PLUGIN_SHUTTING_DOWN_PLUGIN THEN throttle 5.
```
E2: 将错误和警告限制为每小时1000 条,信息消息限制为每小时100 条.
```sql

IF prio <= INFORMATION THEN throttle 1000/3600 ELSE throttle 100/3600.
```
3. set: 为字段赋值(如果字段不存在,则使该字段存在).在后面的规则中,如果此字段在日志中出现则记录

4. unset :丢弃监控的字段 ,跟set 功能相反.

可以指定丢弃哪个字段.

```sql
IF myfield == 2 THEN unset myfield.
IF myfield == 2 THEN unset.
```



#### 规则领域

log_filter_dragnet 支持核心,可选,和用户定义的规则.

为错误事件自动设置核心字段.

- 核心区域字段

time :事件事件

msg :事件消息

prio :事件优先级


```sql
IF prio == INFORMATION THEN ...
IF prio == 3 THEN ...
```

下表显示了符号和数值的对应关系.

| Event Type | Priority Symbol | Numeric Priority |
| --- | --- | --- |
| Error events | `ERROR` | 1 |
| Warning events | `WARNING` | 2 |
| Note/information events | `INFORMATION` | 3 |

Priority values follow the principle that higher priorities have lower values, and vice versa. Priority values begin at 0 for the most severe events (errors) and increase for events with decreasing severity. For example, to discard events with lower priority than warnings, test for priority values higher than WARNING:

```
IF prio > WARNING THEN drop.
```
The following examples show the log_filter_dragnet rules to achieve an effect similar to each log_error_verbosity value permitted by the log_filter_internal filter:

 - 只输出error 

```sql
IF prio > ERROR THEN drop.
```
- 输出 error ,warnings .

```sql

IF prio > WARNING THEN drop.
```
- 输出 error ,warnings ,notes  信息.

```sql

IF prio > INFORMATION THEN drop.
```

####  选项详细说明
##### err_code

数字的错误代码.可以使用符号错误名或者数字.

```

IF err_code == ER_ACCESS_DENIED_ERROR THEN ...
IF err_code == 1045 THEN ...
```
##### err_symbol

事件错误符号,适用于字符串,例如"ER_DUP_KEY" .主要识别日志输出中特定含.

#####  SQL_state

SQLTATE 事件值获取.

##### subsystem

replication 的子系统.相关的事件.

#####  日志的附加信息

 OS_errno :操作系统错误号

 OS_errmsg :操作系统错误消息

 label :事件发生时打标签.

#### 客户端事件

|   选项   |    说明    |
| -------- | --------- |
| user     | 客户端用户 |
| host     | 客户端主机 |
| thread   | 线程       |
| query_id | 查询ID     |
#### debug 信息

source_file :事件所发生的源文件.

```console
IF source_file == "distance.cc" THEN ...
```
source_line :事件发生时源文件中的行.

function :事件发生的函数.

component :事件发生的组件或插件