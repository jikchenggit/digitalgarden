---
{"author":"aming","email":"jikcheng@163.com","title":"Connect Parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 15:08","tags":"Connect Parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/connect-parameter/","dgPassFrontmatter":true}
---








## 登录角色生效参数

### [`activate_all_roles_on_login`]

| Property                  | Value                           |
| ------------------------- | ------------------------------- |
| **Command-Line Format**   | `--activate-all-roles-on-login` |
| **Introduced**            | 8.0.2                           |
| **System Variable**       | `[activate_all_roles_on_login]` |
| **Scope**                 | Global                          |
| **Dynamic**               | Yes                             |
| **[SET_VAR]Hint Applies** | No                              | 
| **Type**                  | boolean                         |
| **Default Value**         | `OFF`                           |


```ad-note
说明:角色管理.
```

此参数在版本8.0.2引入，是一个可以动态调整的global级参数，默认值为OFF。此参数用于控制在账户登录时是否激活已经授予的角色，如果为ON则授予的角色会被激活，设置为OFF时只能通过SET DEFAULT ROLE显式激活用户角色。activate_all_roles_on_login设置只在账户登录或者开始执行存储过程时生效，如果想更改session的role需要执行SET ROLE语句。

详情请见:


[[database/MySQL/Doc/MySQL administration/Mysql MAC Role\|Mysql MAC Role]]




### [`automatic_sp_privileges`]

| Property                  | Value                       |
| ------------------------- | --------------------------- |
| **System Variable**       | `[automatic_sp_privileges]` |
| **Scope**                 | Global                      |
| **Dynamic**               | Yes                         |
| **[SET_VAR]Hint Applies** | No                          | 
| **Type**                  | boolean                     |
| **Default Value**         | `TRUE`                      |



```ad-note
说明:权限管理
```
该参数控制着server是否自动分配execute和alter权限给创建routine的用户。 默认为1，自动赋权
##  认证功能参数
###  [`auto_generate_certs`]

| Property                  | Value                              |
| ------------------------- | ---------------------------------- |
| **Command-Line Format**   | `--auto-generate-certs[={OFF|ON}]` |
| **System Variable**       | `[auto_generate_certs]`            |
| **Scope**                 | Global                             |
| **Dynamic**               | No                                 |
| **[SET_VAR]Hint Applies** | No                                 | 
| **Type**                  | boolean                            |
| **Default Value**         | `ON`                               |


```ad-note
说明:链接认证管理
```


自动认证:当服务启动后服务程序会自动产生server ,client 认证文件,不需要再指定-ssl 选项
例如:一下插件为自动生成,不需要手动指定.
```
sha256_password_auto_generate_rsa_keys and caching_sha2_password_auto_generate_rsa_keys
```
系统变量是相关的，但它控制使用SSL进行安全连接所需的SSL证书和密钥文件的自动生成。

### [`default_authentication_plugin`]

| Property                     | Value                                                           |
| ---------------------------- | --------------------------------------------------------------- |
| **Command-Line Format**      | `--default-authentication-plugin=plugin_name`                   |
| **System Variable**          | `[default_authentication_plugin]`                               |
| **Scope**                    | Global                                                          |
| **Dynamic**                  | No                                                              |
| **[SET_VAR]Hint Applies**    | No                                                              |
| **Type**                     | enumeration                                                     |
| **Default Value** (>= 8.0.4) | `caching_sha2_password`                                         |
| **Default Value** (<= 8.0.3) | `mysql_native_password`                                         |
| **Valid Values** (>= 8.0.3)  | `mysql_native_password``sha256_password``caching_sha2_password` |
| **Valid Values** (<= 8.0.2)  | `mysql_native_password``sha256_password`                        |

更改用户加密方式:
```sql
CREATE USER ... IDENTIFIED BY 'cleartext password';
```

### [`block_encryption_mode`]

| Property                  | Value                       |
| ------------------------- | --------------------------- |
| **Command-Line Format**   | `--block-encryption-mode=#` |
| **System Variable**       | `[block_encryption_mode]`   |
| **Scope**                 | Global, Session             |
| **Dynamic**               | Yes                         |
| **[SET_VAR]Hint Applies** | No                          | 
| **Type**                  | string                      |
| **Default Value**         | `aes-128-ecb`               |

说明:连接加密管理

连接的加密模式:

```
For OpenSSL, permitted mode values are: ECB, CBC, CFB1, CFB8, CFB128, OFB
For wolfSSL, permitted mode values are: ECB, CBC
```



## 连接数参数

### [`back_log`]

|         Property          |      Value       |
| ------------------------- | ---------------- |
| **System Variable**       | `[back_log]`     |
| **Scope**                 | Global           |
| **Dynamic**               | No               |
| **[SET_VAR]Hint Applies** | No               |
| **Type**                  | integer          |
| **Default Value**         | `-1 (autosized)` |
| **Minimum Value**         | `1`              |
| **Maximum Value**         | `65535`          |


```ad-note
说明:连接数管理
```

baklog 选项适用于在非常短的时间内有大量连接.进行指定有多少连接可以进来.


-  修改back_log参数值:由默认的50修改为500.（每个连接256kb, 占用：125M）

`back_log=500`

- 查看mysql 当前系统默认back_log值，命令：

```sql
show variables like 'back_log';
```


  back_log值指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。也就是说，如果MySql的连接数达到max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源。将会报：

` unauthenticated user | xxx.xxx.xxx.xxx | NULL | Connect | NULL | login | NULL` 的待连接进程时.

back_log值不能超过TCP/IP连接的侦听队列的大小。若超过则无效，查看当前系统的TCP/IP连接的侦听队列的大小命令：`cat /proc/sys/net/ipv4/tcp_max_syn_backlog`，目前系统为1024。对于Linux系统推荐设置为大于512的整数。
查看当前系统监听队列:
```console
cat /proc/sys/net/ipv4/tcp_max_syn_backlog
```
修改系统内核参数，可以编辑/etc/sysctl.conf去调整它。
如：
```
net.ipv4.tcp_max_syn_backlog = 2048
```
生效
```console
sysctl -p
```


### [`max_connect_errors`]

| Property                             | Value                                                                        |
| ------------------------------------ | ---------------------------------------------------------------------------- |
| **Command-Line Format**              | `--max-connect-errors=#`                                                     |
| **System Variable**                  | `[max_connect_errors](server-administration.html#sysvar_max_connect_errors)` |
| **Scope**                            | Global                                                                       |
| **Dynamic**                          | Yes                                                                          |
| **[SET_VAR] Hint Applies**           | No                                                                           |
| **Type** (64-bit platforms)          | integer                                                                      |
| **Type** (32-bit platforms)          | integer                                                                      |
| **Default Value** (64-bit platforms) | `100`                                                                        |
| **Default Value** (32-bit platforms) | `100`                                                                        |
| **Minimum Value** (64-bit platforms) | `1`                                                                          |
| **Minimum Value** (32-bit platforms) | `1`                                                                          |
| **Maximum Value** (64-bit platforms) | `18446744073709551615`                                                       | 
| **Maximum Value** (32-bit platforms) | `4294967295`                                                                 |

如果一个客户端多次连续请求都被终端(参数设置为100),服务器将会自动阻止这个客户端进一步连接,直到刷线host_cache  才能进行进一步链接.

详情请见:
[[database/MySQL/Doc/MySQL administration/DNS 解析 和Host Cache\|DNS 解析 和Host Cache]]

##  监听参数
###  [`bind_address`]

| Property                  | Value                 |
| ------------------------- | --------------------- |
| **Command-Line Format**   | `--bind-address=addr` |
| **System Variable**       | `[bind_address]`      |
| **Scope**                 | Global                |
| **Dynamic**               | No                    |
| **[SET_VAR]Hint Applies** | No                    | 
| **Type**                  | string                |
| **Default Value**         | `*`                   |


```ad-note
说明:监听管理
```


此选项为,是在哪个网络接口上进行网络监听.
-  如果是* ,服务监听在本机的所有网卡上.包括IPV4 ,IPV6.
- 如果是`0.0.0.0`,监听所有的IPV4.
- 如果是`::` 服务监听所有的IPV4,IPV6
- 如果是`IPV4` 的映射地址,则要使用`::ffff:127.0.0.1 host=127.0.0.1` or` --host=::ffff:127.0.0.1` 登录
- 如果是特殊的`IPV4` 地址和`IPV6` 地址,你只能从特定的地址进行连接.


## 字符集参数
### [`character_set_client`]

| Property                     | Value                    |
| ---------------------------- | ------------------------ |
| **System Variable**          | `[character_set_client]` |
| **Scope**                    | Global, Session          |
| **Dynamic**                  | Yes                      |
| **[SET_VAR]Hint Applies**    | No                       |
| **Type**                     | string                   |
| **Default Value** (>= 8.0.1) | `utf8mb4`                |
| **Default Value** (8.0.0)    | `utf8`                   |

```ad-note
说明:连接字符集管理
```

数据库客户端默认字符集

###  [`character_set_connection`]

| Property                     | Value                        |
| ---------------------------- | ---------------------------- |
| **System Variable**          | `[character_set_connection]` |
| **Scope**                    | Global, Session              |
| **Dynamic**                  | Yes                          |
| **[SET_VAR]Hint Applies**    | No                           |
| **Type**                     | string                       |
| **Default Value** (>= 8.0.1) | `utf8mb4`                    |
| **Default Value** (8.0.0)    | `utf8`                       |
::: alert-info
说明:客户端字符集
:::
客户端默认字符集



###  [`character_set_results`]

| Property                     | Value                     |
| ---------------------------- | ------------------------- |
| **System Variable**          | `[character_set_results]` |
| **Scope**                    | Global, Session           |
| **Dynamic**                  | Yes                       |
| **[SET_VAR]Hint Applies**    | No                        |
| **Type**                     | string                    |
| **Default Value** (>= 8.0.1) | `utf8mb4`                 |
| **Default Value** (8.0.0)    | `utf8`                    |

```ad-note
说明:查询结果字符集
```







### [`collation_connection`]

| Property                  | Value                    |
| ------------------------- | ------------------------ |
| **System Variable**       | `[collation_connection]` |
| **Scope**                 | Global, Session          |
| **Dynamic**               | Yes                      |
| **[SET_VAR]Hint Applies** | No                       | 
| **Type**                  | string                   |

连接字符集转换.
###  [`collation_database`]

| Property                     | Value                                                                                                                            |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **System Variable**          | `[collation_database]`                                                                                                           |
| **Scope**                    | Global, Session                                                                                                                  |
| **Dynamic**                  | Yes                                                                                                                              |
| **[SET_VAR]Hint Applies**    | No                                                                                                                               |
| **Type**                     | string                                                                                                                           |
| **Default Value** (>= 8.0.1) | `utf8mb4_0900_ai_ci`                                                                                                             |
| **Default Value** (8.0.0)    | `latin1_swedish_ci`                                                                                                              |
| **Footnote**                 | This option is dynamic, but only the server should set this information. You should not set the value of this variable manually. |

数据库字符集转换




















## 密码过期登录参数
###  [`disconnect_on_expired_password`]

| Property                  | Value                                  |
| ------------------------- | -------------------------------------- |
| **Command-Line Format**   | `--disconnect-on-expired-password[=#]` |
| **System Variable**       | `[disconnect_on_expired_password]`     |
| **Scope**                 | Global                                 |
| **Dynamic**               | No                                     |
| **[SET_VAR]Hint Applies** | No                                     | 
| **Type**                  | boolean                                |
| **Default Value**         | `ON`                                   |


```ad-note
对于密码过期的用户连接:
```

对于密码过期的用户登录:
- 如果客户端表明了过期用户的过期密码,则允许连接到一个沙盒环境.
- 如果客户端没有标明过期用户的过期密码,如果
	-  case1:参数打开,则断开连接.
	- case2:允许连接,但是连接到一个沙盒环境中.
	 


## 登录选项设置参数
### [`init_connect`]

| Property                  | Value                 |
| ------------------------- | --------------------- |
| **Command-Line Format**   | `--init-connect=name` |
| **System Variable**       | `[init_connect]`      |
| **Scope**                 | Global                |
| **Dynamic**               | Yes                   |
| **[SET_VAR]Hint Applies** | No                    | 
| **Type**                  | string                |

```ad-note
说明:连接初始化
```


初始化连接时,所执行的命令.命令为字符串.字符串里有一个或者多个命令.使用`;` 分割.例如:自动提交在比较老的服务器上是禁用的(MySQL 5.5.8之前).使用init_connect 可以达到同样的效果.

##  连接超时参数

### [`interactive_timeout`]

| Property                  | Value                     |
| ------------------------- | ------------------------- |
| **Command-Line Format**   | `--interactive-timeout=#` |
| **System Variable**       | `[interactive_timeout]`   |
| **Scope**                 | Global, Session           |
| **Dynamic**               | Yes                       |
| **[SET_VAR]Hint Applies** | No                        | 
| **Type**                  | integer                   |
| **Default Value**         | `28800`                   |
| **Minimum Value**         | `1`                       |


```ad-note
 MySQL 关闭交互式链接所等待的秒数.
```



[[database/MySQL/Doc/MySQL administration/Mysql MAC Role\|Mysql MAC Role]]

## 9.2. [`connect_timeout`]

| Property                  | Value                 |
| ------------------------- | --------------------- |
| **Command-Line Format**   | `--connect-timeout=#` |
| **System Variable**       | `[connect_timeout]`   |
| **Scope**                 | Global                |
| **Dynamic**               | Yes                   |
| **[SET_VAR]Hint Applies** | No                    |
| **Type**                  | integer               |
| **Default Value**         | `10`                  |
| **Minimum Value**         | `2`                   |
| **Maximum Value**         | `31536000`            |


```ad-note
说明:连接等待时间.
```


mysql客户端在尝试与mysql服务器建立连接时，mysql服务器返回错误握手协议前等待客户端数据包的最大时限。默认10秒。
增大这个参数可以减少 `connection to MySQL server at 'XXX', system error: errno.` 这样频繁的报错.
## 网络包大小
### [`max_allowed_packet`]

| Property                     | Value                    |
| ---------------------------- | ------------------------ |
| **Command-Line Format**      | `--max-allowed-packet=#` |
| **System Variable**          | `[max_allowed_packet]`   |
| **Scope**                    | Global, Session          |
| **Dynamic**                  | Yes                      |
| **[SET_VAR] Hint Applies**   | No                       |
| **Type**                     | integer                  |
| **Default Value** (>= 8.0.3) | `67108864`               |
| **Default Value** (<= 8.0.2) | `4194304`                | 
| **Minimum Value**            | `1024`                   |
| **Maximum Value**            | `1073741824`             |

The maximum size of one packet or any generated/intermediate string, or any parameter sent by the mysql_stmt_send_long_data() C API function. The default is 64MB.

## 加密参数
### [`require_secure_transport`]

| Property                   | Value                                   |
| -------------------------- | --------------------------------------- |
| **Command-Line Format**    | `--require-secure-transport[={OFF|ON}]` |
| **System Variable**        | `[require_secure_transport]`            |
| **Scope**                  | Global                                  |
| **Dynamic**                | Yes                                     |
| **[SET_VAR] Hint Applies** | No                                      |
| **Type**                   | boolean                                 | 
| **Default Value**          | `OFF`                                   |

此变量控制了客户端到服务器的连接是否需要使用安全传输.当启用此变量时,服务器只允许使用SSL 的TCP/IP连接,或使用套接字,或者共享内存(windows) ,服务器将拒绝不安全的连接测试,如果客户端连接失败.就会出现一个[`ER_SECURE_TRANSPORT_REQUIRED`] 的错误.

此功能补充了 每个账户都都需要加密连接,例如: 如果用户定义成为了`REQUIRE SSL ` ,开启了这个参数后账户就不能从套接字中登录.

服务器 当然有可能没有启用安全连接,如果启动时没有指定任何SSL证书或密钥文件. 服务器将不支持安全传输.在这样情况之下如果启用`require_secure_transport`  将会报错`ER_NO_SECURE_TRANSPORTS_CONFIGURED`

###  [`tls_version`]

| Property                      | Value                                                          |
| ----------------------------- | -------------------------------------------------------------- |
| **Command-Line Format**       | `--tls-version=protocol_list`                                  |
| **System Variable**           | `[tls_version](server-administration.html#sysvar_tls_version)` |
| **Scope**                     | Global                                                         | 
| **Dynamic**                   | No                                                             |
| **[SET_VAR] Hint Applies**    | No                                                             |
| **Type** (>= 8.0.11)          | string                                                         |
| **Type** (<= 8.0.4)           | string                                                         |
| **Default Value** (>= 8.0.11) | `TLSv1,TLSv1.1,TLSv1.2`                                        |
| **Default Value** (<= 8.0.4)  | `TLSv1,TLSv1.1,TLSv1.2` (OpenSSL)`TLSv1,TLSv1.1` (yaSSL)       |

服务器允许的用于的加密连接的协议, 该值时一个逗号分割列表,包含一个多个协议.