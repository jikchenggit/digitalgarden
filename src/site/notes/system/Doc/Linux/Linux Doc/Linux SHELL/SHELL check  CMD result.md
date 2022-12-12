---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL check  CMD result","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"SHELL check  CMD result","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-check-cmd-result/","dgPassFrontmatter":true}
---



# 1. 检查命令执行成功
```sh
echo $?
```
## 1.1. 检测目录是否存在
```sh
test -d /user/local/bin
if [ "$?" -eq 0 ] # Check the return code
	then # The return code is zero
	echo '/usr/local/bin does exist'
else # The reurn code is NOT zero
	echo '/usr/local/bin does NOT exist'
fi
/usr/local/bin does NOT exist
```

## 1.2. 方式2
```sh
if test -d /usr/local/bin
then    # The return code is zero
        echo '/usr/local/bin does exits'
else    # The return code is NOT zero
        echo '/usr/local/bin does NOT exit'
fi
```
## 1.3. 方式3
```sh
if [ -d /usr/local/bin ]
then    # The reurn code is zero
        echo '/usr/local/bin does exist'
else    # The reurn code is NOT zero
        echo '/usr/local/bin does NOT exist'
fi
```