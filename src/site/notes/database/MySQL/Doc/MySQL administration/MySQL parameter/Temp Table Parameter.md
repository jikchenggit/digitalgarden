---
{"author":"aming","email":"jikcheng@163.com","title":"Temp Table Parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Temp Table Parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/temp-table-parameter/","dgPassFrontmatter":true}
---




## 临时表参数
###  [`avoid_temporal_upgrade`]

|          Property          |                Value                |
| -------------------------- | ----------------------------------- |
| **Command-Line Format**    | `--avoid-temporal-upgrade={OFF|ON}` |
| **Deprecated**             | Yes                                 |
| **System Variable**        | `[avoid_temporal_upgrade]`          |
| **Scope**                  | Global                              |
| **Dynamic**                | Yes                                 |
| **[SET_VAR] Hint Applies** | No                                  |
| **Type**                   | boolean                             |
| **Default Value**          | `OFF`                               |


此变量控制alter table 自动升级临时表,是由于早期的TIME ,DATETIME,TIMESTAMP 列的支持的精度.
默认是不打开,如果打开后为ON ,不会更新所有临时表的列,从而更快的重建临时表.