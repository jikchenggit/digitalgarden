---
{"author":"aming","email":"jikcheng@163.com","title":"File Location parameter","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:05","tags":"File Location parameter","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL parameter","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-parameter/file-location-parameter/","dgPassFrontmatter":true}
---




## 文件管理参数
### [`keep_files_on_create`]

|          Property          |           Value            |
| -------------------------- | -------------------------- |
| **Command-Line Format**    | `--keep-files-on-create=#` |
| **System Variable**        | `[keep_files_on_create]`   |
| **Scope**                  | Global, Session            |
| **Dynamic**                | Yes                        |
| **[SET_VAR] Hint Applies** | No                         |
| **Type**                   | boolean                    |
| **Default Value**          | `OFF`                      |
如果MyISAM 表创建的时候没有指定 DATA DIRRECTORY 选项,则将会在数据库目录下创建一个叫 *.MYD 的文件,如果文件存在则将会覆盖这个文件. 为了阻止这种操作,设置keep_files_on_create 为on ,则MyISAM 不会覆盖文件,而是返回错误.默认值为OFF(0).

### [`large_files_support`]


|          Property          |          Value           |
| -------------------------- | ------------------------ |
| **System Variable**        | `[large_files_support])` |
| **Scope**                  | Global                   |
| **Dynamic**                | No                       |
| **[SET_VAR] Hint Applies** | No                       |

是否开启大文件支持.



## 文件位置参数
###  [`basedir`]

|           Property           |                   Value                   |
| ---------------------------- | ----------------------------------------- |
| **Command-Line Format**      | `--basedir=dir_name`                      |
| **System Variable**          | `[basedir]`                               |
| **Scope**                    | Global                                    |
| **Dynamic**                  | No                                        |
| **[SET_VAR] Hint Applies**   | No                                        |
| **Type**                     | directory name                            |
| **Default Value** (>= 8.0.2) | `parent of mysqld installation directory` |

安装MySQL软件的路径.


### [`datadir`]

|          Property          |        Value         |
| -------------------------- | -------------------- |
| **Command-Line Format**    | `--datadir=dir_name` |
| **System Variable**        | `[datadir]`          |
| **Scope**                  | Global               |
| **Dynamic**                | No                   |
| **[SET_VAR] Hint Applies** | No                   |
| **Type**                   | directory name       |
数据文件目录

### [`lower_case_file_system`]

|          Property          |           Value            |
| -------------------------- | -------------------------- |
| **System Variable**        | `[lower_case_file_system]` |
| **Scope**                  | Global                     |
| **Dynamic**                | No                         |
| **[SET_VAR] Hint Applies** | No                         |
| **Type**                   | boolean                    |

 此变量数据目录所在文件系统的文件名的大小写是否敏感,OFF 表示区分大小写,ON 表示不区分大小写.