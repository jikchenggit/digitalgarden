---
{"author":"aming","email":"jikcheng@163.com","title":"Ambari Install","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"Ambari Install","File Folder with relative path":"database/hadoop/Doc/Install","remark":null,"other":null,"dg-publish":true,"permalink":"/database/hadoop/doc/install/ambari-install/","dgPassFrontmatter":true}
---



## 安装maven
1. 安装必要组件
```
yum install maven wget  rpm-build git npm python2 java  gcc-c++ 
```

npm install -g bower
2. 上传ambari 源码包

```bash
wget https://www-eu.apache.org/dist/ambari/ambari-2.7.5/apache-ambari-2.7.5-src.tar.gz
```


3. 配置 开发环境

```bash
mvn versions:set -DnewVersion=2.7.5.0.0
 
pushd ambari-metrics
mvn versions:set -DnewVersion=2.7.5.0.0
popd
```
