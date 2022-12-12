---
{"author":"aming","email":"jikcheng@163.com","title":"scp","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:11","tags":"scp","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/scp/","dgPassFrontmatter":true}
---



## scp 拷贝目录
```bash
scp -r root@43.224.34.73:/home/lk /root
```

##  scp 免密自动拷贝


1. **首先在备份服务器上配置**

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

2. **在~/.ssh/目录下生成密钥文件**
```bash
ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa
```
3. **然后在其他服务器上配置：**
```bash
mkdir -p ~/.ssh
touch ~/.ssh/authorized_keys
```

4. 将备份服务器的id_rsa.pub内容追加到其他服务器的authorized_keys里面
```bash
ssh 192.168.1.249 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
 

5. 在备份服务器上执行SCP命令
```bash
scp -rp root@192.168.1.248:/tmp/scripts/hello ./
```

```ad-note
到此就可以实现自动输入密码拷贝文件，达到备份其他服务器数据的目的。

```


## scp指定端口号
```bash
scp  -oPort=12321 xcmp.gz dsg@135.148.31.26:/dsg26
```