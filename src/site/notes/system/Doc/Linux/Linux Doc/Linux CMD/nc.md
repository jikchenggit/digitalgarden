---
{"author":"aming","email":"jikcheng@163.com","title":"nc","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"nc","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/nc/","dgPassFrontmatter":true}
---


## 1. 测试端口
```sh
nc -vv 162.30.1.23 5000
```
## 2. 端口扫描
```console

n=256
while [ $n -ge 1 ] 
do 
nc -w 1 -z 192.168.1.$n 3000 >/dev/null 2>&1  
  if [ $? -eq 0 ]  
  then  
    echo 192.168.1.$n:ok  
  else  
    echo 192.168.1.$n:fail
  fi
   let n-=1
done
```