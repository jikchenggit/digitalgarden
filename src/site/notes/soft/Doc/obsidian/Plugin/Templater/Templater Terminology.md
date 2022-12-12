---
{"author":"aming","email":"jikcheng@163.com","title":"Templater Terminology","creation_date":"2022-11-27 12:53","Last modified date":"2022-11-27 18:48","tags":"Templater Terminology","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/templater-terminology/","dgPassFrontmatter":true}
---


要了解模板程序的工作原理，首先需要定义几个术语：
- template(模板)是指：文件中包含的文件。
- command(命令）：是以`<%` 开头，以`%>` 结尾的文本片段。
- function（函数）：可以在命令中调用并返回值的对象。
有两种`function` 函数我们可以调用：
- `internal functions.` :是插件内预定义的函数，例如，`tp.date.now` 是一个返回当前日期的内部函数。
- `User functions`: 用户可以使用`command` 和`user scripts` 自定义函数。
## Example 

以下模板包含2个命令，调用2个不同的内部函数：
```javascript
Yesterday: <% tp.date.yesterday("YYYY-MM-DD") %>
Tomorrow: <% tp.date.tomorrow("YYYY-MM-DD") %>

```