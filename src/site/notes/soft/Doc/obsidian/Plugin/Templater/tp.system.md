---
{"author":"aming","email":"jikcheng@163.com","title":"tp.system","creation_date":"2022-11-27 18:01","Last modified date":"2022-11-27 18:49","tags":"tp.system","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/tp-system/","dgPassFrontmatter":true}
---

## Documentation

### tp.system.clipboard()
显示系统粘贴板中的内容。

### tp.system.prompt
生成提示并返回用户输入。
**语法**
```javascript
tp.system.prompt(prompt_text?: string, default_value?: string, throw_on_cancel: boolean = false, multiline?: boolean = false)
```
**参数**
- default_value： 默认输入的字符串。
- multiline：如果设置为Ture，则输入字段将是多行文本区域。
- prompt_text：出入文本时，提示符。
- throw_on_cancel：如果取消提示，则会引发错误，而不是返回NULL 值。

### tp.system.suggester
生成下拉菜单列表，返回用户选择的项目。
**语法**
```javascript
tp.system.suggester(text_items: string[] ⎮ ((item: T) => string), items: T[], throw_on_cancel: boolean = false, placeholder: string = "", limit?: number = undefined)
```
**参数**
- items: 下拉菜单的数组列表，按照顺序排列。
- limit: 下拉菜单分页显示，提高大型列表的性能。
- placeholder：提示符。
- text_items：字符串属组，每个下拉菜单的显示的文本呢。
- `throw_on_cancel`:如果取消提示，则会引发错误，而不是返回NULL 值。

## Examples
```javascript
Clipboard content: <% tp.system.clipboard() %>

Entered value: <% tp.system.prompt("Please enter a value") %>
Mood today: <% tp.system.prompt("What is your mood today ?", "happy") %>

Mood today: <% tp.system.suggester(["Happy", "Sad", "Confused"], ["Happy", "Sad", "Confused"]) %>
Picked file: [[<% (await tp.system.suggester((item) => item.basename, app.vault.getMarkdownFiles())).basename %>]]


<%*
const execution_value = await tp.system.suggester(["Yes", "No"], ["true", "false"])
%>
Are you using Execution Commands: <%*  tR + execution_value %>


```



详情请参见
[tp.system - Templater (silentvoid13.github.io)](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/system-module.html)