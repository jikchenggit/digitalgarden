---
{"author":"aming","email":"jikcheng@163.com","title":"yum clear","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:04","tags":"yum clear","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux CMD/yum/技巧点","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-cmd/yum//yum-clear/","dgPassFrontmatter":true}
---



* 清理过期cache


```sh
yum clean expire-cache
```
 > Eliminate the local data saying when the metadata and mirrorlists were downloaded for each repo. This means yum will revali‐ .date the cache for each repo. next time it is used. However if the cache is still valid, nothing significant was deleted.


```sh
yum clean packages
```
> Eliminate any cached packages from the system.  Note that packages are not automatically deleted after they are downloaded.

```sh
yum clean headers
```
> Eliminate all of the header files, which old versions of yum used for dependency resolution.

```sh
yum clean metadata
```
> Eliminate all of the files which yum uses to determine the remote availability of packages. Using this option will force yum to download all the metadata the next time it is run.

```sh
yum clean dbcache
```
> Eliminate the sqlite cache used for faster access to metadata.  Using this option will force  yum  to  download  the  sqlite metadata the next time it is run, or recreate the sqlite metadata if using an older repo.

```sh
yum clean rpmdb
```
> Eliminate any cached data from the local rpmdb.

```sh
yum clean plugins
```
> Tell any enabled plugins to eliminate their cached data.

```sh
yum clean all
```
> Does all of the above.