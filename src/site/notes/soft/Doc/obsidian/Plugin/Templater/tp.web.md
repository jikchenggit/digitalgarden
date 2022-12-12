---
{"author":"aming","email":"jikcheng@163.com","title":"tp.web","creation_date":"2022-11-27 18:04","Last modified date":"2022-11-27 18:49","tags":"tp.web","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/tp-web/","dgPassFrontmatter":true}
---

此模块包含所有与web 浏览器相关的内部功能[^1].

## Documentation


### tp.web.daily_quote()
从`https://api.quotable.io` 网站每天获取格言。

### tp.web.random_picture
从`https://unsplash.com/` 获取随机图像。
**语法**
```javascript
tp.web.random_picture(size?: string, query?: string, include_size?: boolean)
```
**参数**

- include_size: 可选参数，用于在图像链接标记中包含指定大小。默认为false。
- query: 将选择与搜索词匹配的照片，多个搜索词可以用逗号分隔。
- size：选择`<width>x<height>` 图像大小


## Examples

```javascript
Web Daily quote: <% tp.web.daily_quote() %> Web Random picture: <% tp.web.random_picture() %> Web Random picture with size: <% tp.web.random_picture("200x200") %> Web random picture with size + query: <% tp.web.random_picture("200x200", "landscape,water") %>
```

[^1]: [tp.web - Templater (silentvoid13.github.io)](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/web-module.html)