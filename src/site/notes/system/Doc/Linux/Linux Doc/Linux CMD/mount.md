---
{"author":"aming","email":"jikcheng@163.com","title":"mount","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"mount","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/mount/","dgPassFrontmatter":true}
---


## 1. mount 命令

挂载语法：
```bash
mount -t type device directory
```
type 类型为：
- vfat：Windows长文件系统。

- ntfs：Windows NT、XP、Vista以及Windows 7中广泛使用的高级文件系统。

- iso9660：标准CD-ROM文件系统。

**`mount`命令的参数**

|    参数     |                         描述                         |
| ----------- | ---------------------------------------------------- |
| `-a`        | 挂载/etc/fstab文件中指定的所有文件系统                 |
| `-f`        | 使`mount`命令模拟挂载设备，但并不真的挂载               |
| `-F`        | 和`-a`参数一起使用时，会同时挂载所有文件系统            |
| `-v`        | 详细模式，将会说明挂载设备的每一步                      |
| `-I`        | 不启用任何/sbin/mount.filesystem下的文件系统帮助文件   |
| `-l`        | 给ext2、ext3或XFS文件系统自动添加文件系统标签           |
| `-n`        | 挂载设备，但不注册到/etc/mtab已挂载设备文件中           |
| `-p *num*`  | 进行加密挂载时，从文件描述符*`num`*中获得密码短语       |
| `-s`        | 忽略该文件系统不支持的挂载选项                         |
| `-r`        | 将设备挂载为只读的                                    |
| `-w`        | 将设备挂载为可读写的（默认参数）                       |
| `-L label`  | 将设备按指定的*`label`*挂载                           |
| `-U *uuid*` | 将设备按指定的*`uuid`*挂载                            |
| `-O`        | 和`-a`参数一起使用，限制命令只作用到特定的一组文件系统上 |
| `-o`        | 给文件系统添加特定的选项                               |

-o参数允许在挂载文件系统时添加一些以逗号分隔的额外选项。以下为常用的选项。

- ro：以只读形式挂载。
- rw：以读写形式挂载。
- user：允许普通用户挂载文件系统。
- check=none：挂载文件系统时不进行完整性校验。
- loop：挂载一个文件。
##  磁盘挂载
```sh
mount /dev/sdb1 /u01
```