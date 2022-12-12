---
{"author":"aming","email":"jikcheng@163.com","title":"tp.frontmatter","creation_date":"2022-11-27 17:41","Last modified date":"2022-11-27 18:48","tags":"tp.frontmatter","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/tp-frontmatter/","dgPassFrontmatter":true}
---



## Documentation
返回前言变量值。
**语法**
```javascript
tp.frontmatter.<frontmatter_variable_name>
```


e.g.
```

File's metadata alias: <% tp.frontmatter.tags %>
Note's type: <% tp.frontmatter["备注"] %>
```