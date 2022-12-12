---
{"author":"aming","email":"jikcheng@163.com","title":"Full-text indexing","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 21:12","tags":"Full-text indexing","File Folder with relative path":"database/MySQL/Doc/MySQL administration","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/full-text-indexing/","dgPassFrontmatter":true}
---






## 创建全文索引 
### 创建测试表
```sql
   CREATE TABLE article ( 
                  id INT AUTO_INCREMENT NOT NULL PRIMARY KEY, 
                  title VARCHAR(200), 
                  body TEXT, 
                  FULLTEXT(title, body) 
              );
```
### 添加全文索引

 - alter table 方式
```sql
ALTER TABLE `article` ADD FULLTEXT INDEX title_idx  (`title`);
ALTER TABLE `article` ADD FULLTEXT body_idx  (`body`);
```
- create 方式
```sql
CREATE FULLTEXT INDEX title_idx ON `article` (`title`);
--也可以在创建索引的时候指定索引的长度:
CREATE FULLTEXT INDEX body_idx ON `article` (`body`(20));
```
- 删除全文索引

```sql
DROP INDEX body_idx ON dsg.article ;
DROP INDEX title_idx ON dsg.article ;`
```
- alter table 方式

```sql
ALTER TABLE dsg.article DROP INDEX title_idx;
ALTER TABLE dsg.article DROP INDEX body_idx;
```
### 插入数据 
```sql
insert into dsg.article (title,body) values('serch','大圣');
insert into dsg.article (title,body) values('serch','齐天大圣');
insert into dsg.article (title,body) values('serch','齐天大圣,孙悟空');
insert into dsg.article (title,body) values('serch','齐天大圣孙悟空');
insert into dsg.article (title,body) values('serch','齐天大圣啊!');
insert into dsg.article (title,body) values('serch','齐天大圣,大圣');
insert into dsg.article (title,body) values('serch','齐天大,圣');
insert into dsg.article (title,body) values('serch','good 孙悟空');
insert into dsg.article (title,body) values('serch','hello 孙悟空');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');
insert into dsg.article (title,body) values('serch','hello');

insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');

insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
insert into dsg.article (title,body) values('serch','good');
```
## 使用全文索引
### 3 使用全文索引的格式：  MATCH (columnName) AGAINST ('string')
    
e.g.:

```sql
SELECT * FROM `student` WHERE MATCH(`name`) AGAINST('聪')
```
- 当查询多列数据时：

::: alert-info
建议在此多列数据上创建一个联合的全文索引，否则使用不了索引的。
SELECT * FROM `student` WHERE MATCH(`name`,`address`) AGAINST('聪 广东')`
:::
::: alert-info

50%的门坎限制(当查询结果很多，几乎所有记录都有，或者极少的数据，都有可能会返回非所期望的结果)

-->可用IN BOOLEAN MODE即可以避开50%的限制。

此时使用全文索引的格式就变成了： 
```sql
SELECT * FROM `student` WHERE MATCH(`name`) AGAINST('聪' IN BOOLEAN MODE)
```
:::

### ft_boolean_syntax (+ -><()~*:""&|)使用的例子：
- \+ : 用在词的前面，表示一定要包含该词，并且必须在开始位置。
 eg: +Apple 匹配：Apple123,     "tommy, Apple"
- \- : 不包含该词，所以不能只用「-yoursql」这样是查不到任何row的，必须搭配其他语法使用。
eg: MATCH (girl_name) AGAINST ('-林志玲 +张筱雨')
- 空(也就是默认情况)，表示可选的，包含该词的顺序较高。 
 例子：

apple banana           找至少包含上面词中的一个的记录行
+apple +juice           两个词均在被包含
+apple macintosh    包含词 “apple”，但是如果同时包含 “macintosh”，它的排列将更高一些
+apple -macintosh   包含 “apple” 但不包含 “macintosh”

###  \> :提高该字的相关性，查询的结果会排在比较靠前的位置。`
###  \< :降低相关性，查询的结果会排在比较靠后的位置。
##  4. 进行查询
- 不使用<>
```sql
mysql> select * from article where match(body) against('齐天大圣' in boolean mode);
+----+-------+------------------------+
| id | title | body                   |
+----+-------+------------------------+
|  6 | serch | 齐天大圣               |
|  7 | serch | 齐天大圣,孙悟空        |
| 10 | serch | 齐天大圣,大圣          |
+----+-------+------------------------+
3 rows in set (0.00 sec)
```
完全匹配比较靠前.

- 使用> 
```sql
mysql> select * from article where match(body) against('齐天大圣 >孙悟空' in boolean mode);
+----+-------+------------------------+
| id | title | body                   |
+----+-------+------------------------+
|  7 | serch | 齐天大圣,孙悟空        |
| 12 | serch | good 孙悟空            |
| 13 | serch | hello 孙悟空           |
|  6 | serch | 齐天大圣               |
| 10 | serch | 齐天大圣,大圣          |
+----+-------+------------------------+
5 rows in set (0.00 sec)
```
发现孙悟空优先级比较高,先显示.
- 使用<
```sql
 mysql> select * from article where match(body) against('齐天大圣 <孙悟空');
+----+-------+------------------------+
| id | title | body                   |
+----+-------+------------------------+
|  7 | serch | 齐天大圣,孙悟空        |
|  6 | serch | 齐天大圣               |
| 10 | serch | 齐天大圣,大圣          |
| 12 | serch | good 孙悟空            |
| 13 | serch | hello 孙悟空           |
+----+-------+------------------------+
5 rows in set (0.00 sec)
```
单独使用< 也是优先级较高.
- 同时使用><

```sql

mysql> select * from article where match(body) against('大圣 >齐天大圣,大圣 <孙悟空 <齐天大圣啊');
+----+-------+------------------------+
| id | title | body                   |
+----+-------+------------------------+
|  7 | serch | 齐天大圣,孙悟空        |
|  9 | serch | 齐天大圣啊!            |
|  6 | serch | 齐天大圣               |
| 10 | serch | 齐天大圣,大圣          |
| 12 | serch | good 孙悟空            |
| 13 | serch | hello 孙悟空           |
+----+-------+------------------------+
6 rows in set (0.00 sec)
```
总结:
::: alert-info

1. 只要使用 ><的总比没用的 靠前；

2. 使用  >的一定比 <的排的靠前 (这就符合相关性提高和降低)；

3. 使用同一类的，使用的越早，排的越前。
:::


### 使用() 调整优先级
 eg: +aaa +(>bbb <ccc) 
找到有aaa和bbb和ccc，aaa和bbb，或者aaa和ccc(因为bbb，ccc前面没有+，所以表示可有可无)，

 然后 aaa&bbb > aaa&bbb&ccc > aaa&ccc
###  ~ :将其相关性由正转负，表示拥有该字会降低相关性，但不像「-」将之排除，只是排在较后面。
    eg:   +apple ~macintosh   先匹配apple，但如果同时包含macintosh，就排名会靠后。
### * :通配符，这个只能接在字符串后面。 
```sql
MATCH (girl_name) AGAINST ('+*ABC*')   #错误，不能放前面
MATCH (girl_name) AGAINST ('+张筱雨*')  #正确
```
### " " :整体匹配，用双引号将一段句子包起来表示要完全相符，不可拆字。 
 eg:  "tommy huang" 可以匹配  tommy huang xxxxx   但是不能匹配  tommy is huang。
### 扩展查询
```sql
 select * from article where match(body) against('good' with query expansion);
+-----+-------+------------------------+
| id  | title | body                   |
+-----+-------+------------------------+
|  12 | serch | good 孙悟空            |
|   7 | serch | 齐天大圣,孙悟空        |
|  13 | serch | hello 孙悟空           |
|  66 | serch | good                   |
|  67 | serch | good                   |
|  68 | serch | good                   |
|  69 | serch | good                   |
|  70 | serch | good                   |
|  71 | serch | good                   |
|  72 | serch | good                   |
```
结果里包含孙悟空.
