---
{"author":"aming","email":"jikcheng@163.com","title":"Session TimeOut","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-28 14:46","tags":"Session TimeOut","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/session-time-out/","dgPassFrontmatter":true}
---









# wait_time 修改
## 原理说明:

```bash
set global interactive_timeout=288000;
```
> wait_time 要根据 interactive_timeout 的值改变而改变.
在线程启动时，会话wait_timeout值由全局wait_timeout值或全局interactive_timeout值初始化，这取决于客户机的类型(由mysql_real_connect()的CLIENT_INTERACTIVE connect选项定义)。也看到interactive_timeout。

> interactive_timeout

服务器在关闭交互连接之前等待活动的秒数。交互式客户机被定义为使用mysql_real_connect()的CLIENT_INTERACTIVE选项的客户机。也看到wait_timeout跟随着变化。

命令列表
```bash
set global interactive_timeout=288000;
```
查看是否生效.
```bash
show variables where variable_name in ('interactive_timeout','wait_timeout');
```