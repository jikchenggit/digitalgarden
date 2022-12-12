---
{"author":"aming","email":"jikcheng@163.com","title":"Windows  edit tasks","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:01","tags":"Windows  edit tasks","File Folder with relative path":"system/Doc/WINDOWS/windows Configer","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/windows/windows-configer/windows-edit-tasks/","dgPassFrontmatter":true}
---



```powershell
forfiles /p "D:\dataxone\hjls_kafka\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\hztj_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\hztj_kafka_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\cpjdgl01_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\zddw_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\xfjdgl_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mhjy_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\cpjdgl_kafka\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\zddw_kafka\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\xfjdgl_kafka\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mhjy_kafka\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\cpjdgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\hztj2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\iam2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\jwgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\mhjy2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\wsgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\xfjdgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\yfgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\zbgl2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\zddw2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\zhywpt2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mpp\zzgz2mpp_source\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\cpjdgl2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\hjsl2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\hztj2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\mhjy2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\xfjdgl2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\yfgl2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\zbgl2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\zddw2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
forfiles /p "D:\dataxone\mysql\zzgz2mysql_ds\log\archivelog" /m log* -d -7 /c "cmd /c del /f @path"
```