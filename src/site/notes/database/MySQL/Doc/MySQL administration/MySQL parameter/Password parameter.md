---
{"author":"aming","email":"jikcheng@163.com","title":"Password parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"Password parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/password-parameter/","dgPassFrontmatter":true}
---




## 密码管理参数
###  [`default_password_lifetime`]

|         Property          |              Value              |
| ------------------------- | ------------------------------- |
| **Command-Line Format**   | `--default-password-lifetime=#` |
| **System Variable**       | `[default_password_lifetime]`   |
| **Scope**                 | Global                          |
| **Dynamic**               | Yes                             |
| **[SET_VAR]Hint Applies** | No                              |
| **Type**                  | integer                         |
| **Default Value**         | `0`                             |
| **Minimum Value**         | `0`                             |
| **Maximum Value**         | `65535`                         |

在MySQL 5.6 之后添加的密码生命周期:

[[database/MySQL/Doc/MySQL administration/MySql User Password Expiration\|MySql User Password Expiration]]