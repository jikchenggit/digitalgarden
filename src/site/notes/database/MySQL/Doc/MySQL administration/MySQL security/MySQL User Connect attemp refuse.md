---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL User Connect attemp refuse","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"MySQL User Connect attemp refuse","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL security","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-security/my-sql-user-connect-attemp-refuse/","dgPassFrontmatter":true}
---




## 控制用户登录次数
为了MySQL 链接控制失败的次数，当一个账户失败次数超过一定设置，则链接延迟等待控制为多少。
### 配置参数
```bash
-- 配置文件增加以下配置
[mysqld]
plugin-load-add									= connection_control.so
connection-control                             	= FORCE
connection-control-failed-login-attempts       	= FORCE
connection_control_min_connection_delay			= 1000
connection_control_max_connection_delay			= 86400
connection_control_failed_connections_threshold	= 3
```


### 插件动态安装启用
```sql
mysql> INSTALL PLUGIN CONNECTION_CONTROL SONAME 'connection_control.so';
mysql> INSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS SONAME 'connection_control.so';
```

### 验证是否正常安装
```sql
mysql> SELECT PLUGIN_NAME, PLUGIN_STATUS 
FROM INFORMATION_SCHEMA.PLUGINS
WHERE PLUGIN_NAME LIKE 'connection%';
mysql> SHOW PLUGINS;
```
```
# 参数说明：
- connection_control_failed_connections_threshold

>  允许账户失败次数。如果超过这个设置此时，则以下两个参数生效。
connection_control_max_connection_delay
>  登录失败后，最大的延迟时间。

connection_control_min_connection_delay

> 登录失败后，最小的延迟时间。
例子： 如 `connection_control_failed_connections_threshold` 设置为3 .
connection_control_max_connection_delay 设置为3000.
connection_control_min_connection_delay 设置为1000.
第四次登录失败，延迟1S。
第五次登录失败，延迟2S。
第六次登录失败，延迟3S。
第七次登录失败，延迟3S。
