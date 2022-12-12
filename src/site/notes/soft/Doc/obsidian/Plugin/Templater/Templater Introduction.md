---
{"author":"aming","email":"jikcheng@163.com","title":"Templater Introduction","creation_date":"2022-11-27 12:52","Last modified date":"2022-11-27 18:48","tags":"Templater Introduction","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/templater-introduction/","dgPassFrontmatter":true}
---


## Templater Introduction 
`Templater` 是一种模板语言，可以将变量和函数结果插入到笔记中。还可以使用`JavaScripts` 代码操作这些变量和函数。

`Templater` 能够自动或手动执行相关任务。

## Templater Example
1. 下面面就是`Templater` 语法
```javascript
---
creation date: <% tp.file.creation_date() %>
modification date: <% tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm:ss") %>
---

<< [[<% tp.date.now("YYYY-MM-DD", -1) %>]] | [[<% tp.date.now("YYYY-MM-DD", 1) %>]] >>

# <% tp.file.title %>

<% tp.web.daily_quote() %>

```
1. 使用插入命令后会产生以下命令
```
---
creation date: 2021-01-07 17:20
modification date: Thursday 7th January 2021 17:20:43
---

<< [[2021-04-08]] | [[2021-04-10]] >>

# Test Test

> Do the best you can until you know better. Then when you know better, do better.
> &mdash; <cite>Maya Angelou</cite>

```