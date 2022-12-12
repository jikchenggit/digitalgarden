---
{"author":"aming","email":"jikcheng@163.com","title":"sed","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"sed","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/sed/","dgPassFrontmatter":true}
---



##  sed 字符串替换
### sed替换的基本语法为:

```bash
sed 's/原字符串/替换字符串/'
```
> 单引号里面,s表示替换,三根斜线中间是替换的样式,特殊字符需要使用反斜线”\”进行转义。

> 单引号” ‘ ’”是没有办法用反斜线”\”转义的,这时候只要把命令中的单引号改为双引号就行了,格式如下：

### 要处理的字符包含单引号
```bash
sed "s/原字符串包含'/替换字符串包含'/" 
```
>  命令中的三根斜线分隔符可以换成别的符号,有时候替换目录字符串的时候有较多斜线，这个时候换成其它的分割符是较为方便,只需要紧跟s定义即可。


###  将分隔符换成问号”?”:
```bash
sed 's?原字符串?替换字符串?'
```

> 可以在末尾加g替换每一个匹配的关键字,否则只替换每行的第一个,例如:


### 替换所有匹配关键字
```bash
sed 's/原字符串/替换字符串/g'
```
### 一些特殊字符的使用

”^”表示行首

”$”符号如果在引号中表示行尾，但是在引号外却表示末行(最后一行)


>  注意这里的 " & " 符号，如果没有 “&”，就会直接将匹配到的字符串替换掉
```bash
sed 's/^/添加的头部&/g' 　　　　 #在所有行首添加
sed 's/$/&添加的尾部/g' 　　　　 #在所有行末添加
sed '2s/原字符串/替换字符串/g'　 #替换第2行
sed '$s/原字符串/替换字符串/g'   #替换最后一行
sed '2,5s/原字符串/替换字符串/g' #替换2到5行
sed '2,$s/原字符串/替换字符串/g' #替换2到最后一行
```

 

###  批量替换字符串


```bash
sed -i "s/查找字段/替换字段/g" `grep 查找字段 -rl 路径`
sed -i "s/oldstring/newstring/g" `grep oldstring -rl yourdir
```

>  sed处理过的输出是直接输出到屏幕上的,使用参数”i”直接在文件中替换。


### 替换文件中的所有匹配项
```bash
sed -i 's/原字符串/替换字符串/g' filename
```
>  多个替换可以在同一条命令中执行,用分号”;”分隔，其格式为:

###  同时执行两个替换规则
```bash
sed 's/^/添加的头部&/g；s/$/&添加的尾部/g' 
```
###  删除空行
```bash
sed -i '/^*$/d' ./600.txt # 删除空行
```
### 删除行尾空格
```bash
sed -i 's/ *$//' ./600.txt # 删除行位空格
```
### 行尾添加
```bash
sed -i 's/$/&;/g' ./600.txt　　 #在所有行末添加
```
###  行首添加
```bash
sed -i 's/^/select first 1 * from  &/g' ./600.txt　　　　 #在所有行首添加
```

###  删除匹配行

```bash
sed -i '/SQL>/d' ./600.txt # 删除匹配行
```
###  删除换行符
```bash
sed ':a;N;s/\n/ /g;ta' filename.dat > log.a
```
## 看某行数据

```bash
` sed -n '5,10p' filename 
```
 这样你就可以只查看文件的第5行到第10行。

### 提出单引号中的内容
```bash

sed -r 's/.*'(.+).*/\1/' awk   '{print $1}'`
```

### 替换conf 文件
```bash
sed -i "s/recovery='automatic'/recovery='standby'/"  $KINGBASE_DATA/../etc/repmgr.conf
```