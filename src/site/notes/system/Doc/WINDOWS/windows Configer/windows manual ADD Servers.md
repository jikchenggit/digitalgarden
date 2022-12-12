---
{"author":"aming","email":"jikcheng@163.com","title":"windows manual ADD Servers","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 18:31","tags":"windows manual ADD Servers","File Folder with relative path":"system/Doc/WINDOWS/windows Configer","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/windows/windows-configer/windows-manual-add-servers/","dgPassFrontmatter":true}
---



```bat
echo "创建服务"
sc create Svnservice binpath= "C:\Program Files\Subversion\bin\svnserve.exe --service -r E:\SVNRoot" displayname= "SvnService" depend= Tcpip
echo "删除服务"
sc delete SvnService
echo "修改配置"
sc config Svnservice binpath= "C:\Program Files\Subversion\bin\svnserve.exe --service -r E:\SVNRoot" displayname= "SvnService" depend= Tcpip
echo "设置为自启动"
sc config SvnService start= auto
echo "启动服务"
net start SvnService
```