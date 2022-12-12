---
{"author":"aming","email":"jikcheng@163.com","title":"kingbaseHA Soft Get","creation_date":"2022-11-01 17:07","Last modified date":"2022-11-27 18:59","tags":"kingbaseHA Soft Get","File Folder with relative path":"database/Kingbase/Doc/Cluster manager/RAC/Doc","remark":null,"other":null,"dg-publish":true,"permalink":"/database/kingbase/doc/cluster-manager/rac/doc/kingbase-ha-soft-get/","dgPassFrontmatter":true}
---





## 绿色版安装包部署
```ad-warning
以下命令都为root 用户执行。
以下命令所有节点都需执行。
```


### 获取安装包方式

| 版本         | 路径                                                                                |
| ------------ | ----------------------------------------------------------------------------------- |
| KESV8R6C5B23 | /opt/KingbaseHA                                                                  |
| KESV8R6C6B13 | $KINGBASE_HOME/KESRealPro/V008R006C006B0013/install/script/rootDeployClusterware.sh |
| KESV9        | 待定                                                                                | 

#### KESV8R6C5B23 
```bash
tar -zcvf  KingbaseHA.tar.gz /opt/KingbaseHA
```
#### KESV8R6C6B13
root 
```bash
cd $KINGBASE_HOME/KESRealPro/V008R006C006B0013/install/script/
./rootDeployClusterware.sh
cd /opt
tar -zcvf  KingbaseHA.tar.gz /opt/KingbaseHA
```

```ad-warning
不同版本的KingbaseHA 生成方式都不同，详情参考[官方文档](https://help.kingbase.com.cn/v8/highly/Clusterware/clusterware/index.html)。
1. 必须使用root 用户。
2. 必须使用`./rootDeployClusterware.sh`这种方式生成。
```
### 上传安装包并部署
```bash
cd /opt
tar -zxvf KingbaseHA.tar.gz
```