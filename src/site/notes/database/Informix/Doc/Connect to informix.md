---
{"author":"aming","email":"jikcheng@163.com","title":"Connect to informix","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Connect to informix","File Folder with relative path":"database/Informix/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/informix/doc/connect-to-informix/","dgPassFrontmatter":true}
---


## 连接informix 数据库

DbVisualizer是一个完全基于JDBC的跨平台数据库管理工具，内置SQL语句编辑器（支持语法高亮），凡是具有JDBC数据库接口的数据库都可以管理。
工具:
DbVisualizer
informix数据库jdbc驱动包 ifxjdbc.jar
1. 打开DbVisualizer软件，点击"Tools"菜单，选择"Connection wizard"选项，进入配置窗口"New Connection Wizard"
![](_v_images/20190328115829825_4938.png =602x)
2. 在"New Connection Wizard"窗口中，输入一个连接名称，可随意取名，之后点击"Next"
![](_v_images/20190328115741497_6667.png =600x)
3. 在"Select Database Driver"驱动选择步骤中选择Informix驱动，前提是在DbVisualizer安装目录下的lib目录有ifxjdbc.jar驱动包，没有可在网上下载。点击"Next"
![](_v_images/20190328115905753_14718.png =594x)
4. 设置数据库信息

|       参数        |           说明           |
| ----------------- | ------------------------ |
| Database Passwoed | 数据库连接密码，必须设置   |
| Database Server   | 数据库服务器IP，必须设置   |
| Database Port     | 数据库服务器端口，必须设置 |
| Database          | 数据库名，必须设置        |
| Database Server   | 数据库实例名，必须设置     |
| Database Userid   | 数据库连接名，必须设置     |
- 检测是否正常
在上一步的设置窗口中，点击"Ping Server"按钮，测试是否能ping通数据库服务器，测试通过则点击"Finish"完成按钮，失败则检查配置情况