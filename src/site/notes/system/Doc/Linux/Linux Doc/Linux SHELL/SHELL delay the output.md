---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL delay the output","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"SHELL delay the output","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-delay-the-output/","dgPassFrontmatter":true}
---


```sh
#!/bin/bash
OUTFILE="/tmp/outfile.out" # Define the output file
cat /dev/null > $OUTFILE # Create a zero size output file
# 2. Start an until loop to catch the delayed reponse
until [ -s $OUTFILE ]
do 
        echo $SHELL >> $OUTFILE
done
# 3. Show the resulting output

more $OUTFILE

```