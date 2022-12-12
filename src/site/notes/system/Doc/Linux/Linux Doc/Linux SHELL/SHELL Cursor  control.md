---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL Cursor  control","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL Cursor  control","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-cursor-control/","dgPassFrontmatter":true}
---



# 1. 光标行操控
| KSH | 光标控制|
| --- | --- |
| echo |  控制行移动 |
| \n |  移动新的一行 |
| \c | 控制光标继续在本行|
| \b | 控制光标后退 |
| \t | 控制光标一个制表位|
| \r | 控制光标返回|
| \v | 控制光标垂直移动|
| BASH| 光标控制|
要多加个 -e 
例如：
echo -e "\n"
确定行操作的规范
```sh
# 2. Set up the corret echo command usage. Many Linux
# 3. distributions will execute in Bash even if the 
# 4. script specifiles Korn shell. Bash shell requires
# 5. we use echo -e when we use \n, \c , etc.
case $SHELL in 
*/bin/Bash) alias echo="echo -e"
            ;;
esac
```