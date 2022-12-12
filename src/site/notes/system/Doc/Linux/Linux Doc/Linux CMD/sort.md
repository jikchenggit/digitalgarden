---
{"author":"aming","email":"jikcheng@163.com","title":"sort","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"sort","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/sort/","dgPassFrontmatter":true}
---


## 帮助列表

| 单破折线 |            双破折线            |                                    描述                                     |
| ------- | ----------------------------- | -------------------------------------------------------------------------- |
| `-b`    | `--ignore-leading-blanks`     | 排序时忽略起始的空白                                                         |
| `-C`    | `--check=quiet`               | 不排序，如果数据无序也不要报告                                                |
| `-c`    | `--check`                     | 不排序，但检查输入数据是不是已排序；未排序的话，报告                           |
| `-d`    | `--dictionary-order`          | 仅考虑空白和字母，不考虑特殊字符                                              |
| `-f`    | `--ignore-case`               | 默认情况下，会将大写字母排在前面；这个参数会忽略大小写                          |
| `-g`    | `--general-number-sort`       | 按通用数值来排序（跟`-n`不同，把值当浮点数来排序，支持科学计数法表示的值）       |
| `-i`    | `--ignore-nonprinting`        | 在排序时忽略不可打印字符                                                     |
| `-k`    | `--key=*POS1*[,*POS2*]`       | 排序从POS1位置开始；如果指定了POS2的话，到POS2位置结束                         |
| `-M`    | `--month-sort`                | 用三字符月份名按月份排序                                                     |
| `-m`    | `--merge`                     | 将两个已排序数据文件合并                                                     |
| `-n`    | `--numeric-sort`              | 按字符串数值来排序（并不转换为浮点数）                                        |
| `-o`    | `--output=*file*`             | 将排序结果写出到指定的文件中                                                  |
| `-R`    | `--random-sort`               | 按随机生成的散列表的键值排序                                                  |
|         | `--random-source=*FILE*`      | 指定`-R`参数用到的随机字节的源文件                                            |
| `-r`    | `--reverse`                   | 反序排序（升序变成降序）                                                     |
| `-S`    | `--buffer-size=*SIZE*`        | 指定使用的内存大小                                                           |
| `-s`    | `--stable`                    | 禁用最后重排序比较                                                           |
| `-T`    | `--temporary-directory=*DIR*` | 指定一个位置来存储临时工作文件                                                |
| `-t`    | `--field-separator=*SEP*`     | 指定一个用来区分键位置的字符                                                  |
| `-u`    | `--unique`                    | 和`-c`参数一起使用时，检查严格排序；不和`-c`参数一起用时，仅输出第一例相似的两行 |
| `-z`    | `--zero-terminated`           | 用NULL字符作为行尾，而不是用换行符                                            |
##  去除重复行
它的作用很简单，就是在输出行中去除重复行。

```sh
[rocrocket@rocrocket programming]$ cat seq.txt
banana
apple
pear
orange
pear
[rocrocket@rocrocket programming]$ sort seq.txt

apple
banana
orange
pear
pear
[rocrocket@rocrocket programming]$ sort -u seq.txt
apple
banana
orange
pear
```
## 按照每列关键字去重

```console
sort  -t ' ' -k 1,3 -u 
```

  pear由于重复被-u选项无情的删除了。