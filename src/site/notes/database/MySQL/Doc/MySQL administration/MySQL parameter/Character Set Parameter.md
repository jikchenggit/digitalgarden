---
{"author":"aming","email":"jikcheng@163.com","title":"Character Set Parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:06","tags":"Character Set Parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/character-set-parameter/","dgPassFrontmatter":true}
---




## 数据库字符集参数
###  [`character_set_database`]

|           Property           |                                                              Value                                                               |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **System Variable**          | `[character_set_database])`                                                                                                      |
| **Scope**                    | Global, Session                                                                                                                  |
| **Dynamic**                  | Yes                                                                                                                              |
| **[SET_VAR] Hint Applies**   | No                                                                                                                               |
| **Type**                     | string                                                                                                                           |
| **Default Value** (>= 8.0.1) | `utf8mb4`                                                                                                                        |
| **Default Value** (8.0.0)    | `latin1`                                                                                                                         |
| **Footnote**                 | This option is dynamic, but only the server should set this information. You should not set the value of this variable manually. |
数据库字符集

### [`character_set_filesystem`]

|          Property          |               Value               |
| -------------------------- | --------------------------------- |
| **Command-Line Format**    | `--character-set-filesystem=name` |
| **System Variable**        | `[character_set_filesystem]`      |
| **Scope**                  | Global, Session                   |
| **Dynamic**                | Yes                               |
| **[SET_VAR] Hint Applies** | No                                |
| **Type**                   | string                            |
| **Default Value**          | `binary`                          |
导出文件的字符集,如果系统为UTF8 则为UTF8 ,默认二进制.

### [`character_set_server`]

|           Property           |          Value           |
| ---------------------------- | ------------------------ |
| **Command-Line Format**      | `--character-set-server` |
| **System Variable**          | `[character_set_server]` |
| **Scope**                    | Global, Session          |
| **Dynamic**                  | Yes                      |
| **[SET_VAR] Hint Applies**   | No                       |
| **Type**                     | string                   |
| **Default Value** (>= 8.0.1) | `utf8mb4`                |
| **Default Value** (8.0.0)    | `latin1`                 |
数据库字符集
### [`character_set_system`]

|      Property       |          Value           |
| ------------------- | ------------------------ |
| **System Variable** | `[character_set_system]` |
| **Scope**           | Global                   |
| **Dynamic**         | No                       |
| **Type**            | string                   |
| **Default Value**   | `utf8`                   |
数据库系统目录字符集


###  [`character_sets_dir`]

|          Property          |              Value              |
| -------------------------- | ------------------------------- |
| **Command-Line Format**    | `--character-sets-dir=dir_name` |
| **System Variable**        | `[character_sets_dir]`          |
| **Scope**                  | Global                          |
| **Dynamic**                | No                              |
| **[SET_VAR] Hint Applies** | No                              |
| **Type**                   | directory name                  |
字符集的安装目录