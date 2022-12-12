---
{"author":"aming","email":"jikcheng@163.com","title":"utools Install directory Change","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 20:26","tags":"utools Install directory Change","File Folder with relative path":"soft/Doc/utools","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/utools/utools-install-directory-change/","dgPassFrontmatter":true}
---



1. 退出uTools
2. `win+R` 后输入 `%APPDATA%`，找到`uTools`文件夹（假设目前uTools文件夹的路径是 `C:\Users\jick\AppData\Roaming\uTools`）
3. 选中uTools文件夹，剪切到你想要的任何位置（假设剪切后uTools文件夹的路径为 `D:\SynologyDrive\uTools`）
4. win+R 后输入 cmd，打开cmd窗口
5. 在cmd窗口中输入
```bash
mklink /d "C:\Users\jikch\AppData\Roaming\uTools" "D:\SynologyDrive\uTools"，回车
```
8. （记得替换路径）

完成上述操作后，你会发现原来的AppData目录中出现了一个类似快捷方式的uTools目录，但是仅仅是类似，实际上是创建了一个软链接，普通的快捷方式是没法达到这个目的的。

这样就能让uTools不占用C盘空间，将数据储存到其他地方了。
```ad-note
理论上linux和mac应该就是把mklink /d换成ln -s就行了
```
