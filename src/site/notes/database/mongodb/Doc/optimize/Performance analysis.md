---
{"author":"aming","email":"jikcheng@163.com","title":"Performance analysis","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:04","tags":"Performance analysis","File Folder with relative path":"database/mongodb/Doc/optimize","remark":null,"other":null,"dg-publish":true,"permalink":"/database/mongodb/doc/optimize/performance-analysis/","dgPassFrontmatter":true}
---


## 打开数据库profile 性能分析功能

```bash
db.setProfilingLevel(2)

{ "was" : 0, "slowms" : 100, "sampleRate" : 1.0, "ok" : 1 }
```