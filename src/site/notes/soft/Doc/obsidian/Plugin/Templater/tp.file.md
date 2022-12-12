---
{"author":"aming","email":"jikcheng@163.com","title":"tp.file","creation_date":"2022-11-27 15:34","Last modified date":"2022-11-27 18:48","tags":"tp.file","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/tp-file/","dgPassFrontmatter":true}
---



## File Module
### Documentation Syntax
函数语法规则，请参见[[soft/Doc/obsidian/Plugin/Templater/Templater Syntax\|Templater Syntax]]

### tp.file.content
返回文件内容。
```javascript
<% tp.file.content %>
```

### tp.file.create_new

使用指定的门口版或指定的内容创建新文件。
**语法**
```javascript
tp.file.create_new(template: TFile ⎮ string, filename?: string, open_new: boolean = false, folder?: TFolder)
```
**参数**
- filename: 新创建额文件名，默认为`Untitled`.
- folder: 创建新文件默认的存储位置，如果没有指定则是obsidian 的默认位置[^1]。
- open_new: 是否打开新创建的文件。
- template: 创建新文件所使用的字符串和内容，如果要使用模板，使用tp.file_tfile(TEMPLATENAME) 进行检索
```ad-warning
如果你开启此选项，由于命令执行是异步的。因此文件可以先打开，然后将其他命令追加到新文件而不是以前的文件。
```
- template：使用哪个模板作为新文件的内容。

[^1]: [tp.file - Templater (silentvoid13.github.io)](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/file-module.html#tpfilecreate_newtemplate-tfile--string-filename-string-open_new-boolean--false-folder-tfolder)

### tp.file.creation_date
返回文件的创建日期。
**语法**
```
tp.file.creation_date(format: string = "YYYY-MM-DD HH:mm")
```
**参数**
- format: 日期的格式请参见[[soft/Doc/obsidian/Plugin/Templater/tp.date#Moment.js\|tp.date#Moment.js]].
## tp.file.cursor

**语法**
```
tp.file.cursor(order?: number)
```
插入模板内容后，设置光标的位置。

可以在obsidian 中设置快捷键。

**参数**
- order: 设置光标的位置，1，2，3等。
## tp.file.cursor_append
在活动的光标后面添加一些内容。

**语法**
```javascript
tp.file.cursor_append(content: string)
```
**参数**
- content: 活动光标后添加的内容。
## tp.file.exists
检查文件是否存在，返回完整的路径名。
**语法**
```javascript
tp.file.exists(filename: string)
```
**参数**
- filename: 笔记的文件名，e.g. MyFile。

##  tp.file.find_tfile
搜寻模板文件，并创建一个以这个模板文件创建的笔记。

**语法**
```javascript
tp.file.find_tfile(filename: string)
```
**参数**
-filename: 要搜索模板文件名。

## tp.file.folder
返回文件所在的目录路径。

**语法**
```javascript
tp.file.folder(relative: boolean = false)
```
**参数**
relative: 如果设置为`true` ，则将显示相对路径。

## tp.file.include
导入其他文件的内容。导入模板时内容将会被替换。


**语法**

```javascript
tp.file.include(include_link: string ⎮ TFile)
```
**参数**
include_link: 指向包含文件内容的链接，e.g. [[myFile\|myFile]],还支持部分导入，e.g. [[myFile#part 1\|myFile#part 1]]
## tp.file.last_modified_date
返回文件最后一次修改的日期
**语法**
```
tp.file.last_modified_date(format: string = "YYYY-MM-DD HH:mm")
```
**参数**
-  format ： 指定日期的格式。
## tp.file.move
移动文件到另外的目录。
**语法**
```javascript
tp.file.move(new_path: string, file_to_move?: TFile)
```
**参数**

- new_path: 新文件的相对路径，不要带文件扩展名。
## tp.file.path
返回文件的绝对路径。
**语法**
```javascript
tp.file.path(relative: boolean = false)
```
**参数**
- relative：如果参数为真，则返回相对路径。
## tp.file.rename
重命名文件。
**语法**
```javascript
tp.file.rename(new_title: string)
```
**参数**
- new_title: 新的文件名。
## tp.file.selection

活动文本的选择。


```javascript
tp.file.selection()
```

## tp.file.tags
返回笔记的标签。


```javascript
tp.file.tags
```


## tp.file.title

返回文件的名称。
## EXAMPLE
```javascript
File content: <% tp.file.content %>

File creation date: <% tp.file.creation_date() %>
File creation date with format: <% tp.file.creation_date("dddd Do MMMM YYYY HH:mm") %>

File creation: [[<% (await tp.file.create_new("MyFileContent", "MyFilename")).basename %>]]

File cursor: 

File cursor append: <% tp.file.cursor_append("Some text") %>
    
File existence: <% await tp.file.exists("MyFolder/MyFile.md") %>
File existence of current file: <% await tp.file.exists(tp.file.folder(true)+"/"+tp.file.title+".md") %>

File find TFile: <% tp.file.find_tfile("MyFile").basename %>
    
File Folder: <% tp.file.folder() %>
File Folder with relative path: <% tp.file.folder(true) %>

File Include: <% tp.file.include("[[Template1]]") %>

File Last Modif Date: <% tp.file.last_modified_date() %>
File Last Modif Date with format: <% tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm") %>

File Move: <% await tp.file.move("/A/B/" + tp.file.title) %>
File Move + Rename: <% await tp.file.move("/A/B/NewTitle") %>

File Path: <% tp.file.path() %>
File Path with relative path: <% tp.file.path(true) %>

File Rename: <% await tp.file.rename("MyNewName") %>
Append a "2": <% await tp.file.rename(tp.file.title + "2") %>

File Selection: <% tp.file.selection() %>

File tags: <% tp.file.tags %>

File title: <% tp.file.title %>
Strip the Zettelkasten ID of title (if space separated): <% tp.file.title.split(" ")[1] %>

```