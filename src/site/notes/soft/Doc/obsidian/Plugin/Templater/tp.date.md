---
{"author":"aming","email":"jikcheng@163.com","title":"tp.date","creation_date":"2022-11-27 14:47","Last modified date":"2022-11-27 18:48","tags":"tp.date","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/tp-date/","dgPassFrontmatter":true}
---






## date Module 

### Documentation 
#### tp.date.now 
**语法：**
```javascript
tp.date.now(format: string = "YYYY-MM-DD", offset?: number⎮string, reference?: string, reference_format?: string)
```
**参数：**

| 参数             | 说明                                    |
| ---------------- | --------------------------------------- |
| format           | 日期格式                                |
| offset           | 日期偏移量，例如设置`-7` 取上周的日志。 |
| reference        | 日期的引用，可以将日志作为笔记的标题 。 |
| reference_format | 日期的引用格式。                        | 

`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)
### tp.date.tomorrow
返回明天的日期。
**语法：**
```
tp.date.tomorrow(format: string = "YYYY-MM-DD")
```
**参数：**
| 参数             | 说明                                    |
| ---------------- | --------------------------------------- |
| format           | 日期格式                                |
`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)

### tp.date.weekday
返回星期几,是哪一个天。

```javascript
tp.date.weekday(format: string = "YYYY-MM-DD", weekday: number, reference?: string, reference_format?: string)
```
**参数：**

| 参数             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| format           | 日期格式                                                 |
| weekday          | 一周编号，星期一为第一天，编号为`0`，`-7` 上周的第一天。 |
| reference        | 日期的引用，可以将日志作为笔记的标题 。                  |
| reference_format | 日期的引用格式。                                         |
**示例**
```javascripts
 <% tp.date.weekday( "YYYY-MM-DD",0) %>  
```
![](https://www.aming.work:8084/images/2022/11/27/20221127152433.png)

## tp.date.yesterday

返回昨天的日期
**参数：**

| 参数             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| format           | 日期格式                                                 |
`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)



## Moment.js
`templater` 可以访问`moment` 对象。获取更多的函数。请参见[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/)
## Examples
```javascript
Date now: <% tp.date.now() %>
Date now with format: <% tp.date.now("Do MMMM YYYY") %>

Last week: <% tp.date.now("dddd Do MMMM YYYY", -7) %>
Today: <% tp.date.now("dddd Do MMMM YYYY, ddd") %>
Next week: <% tp.date.now("dddd Do MMMM YYYY", 7) %>

Last month: <% tp.date.now("YYYY-MM-DD", "P-1M") %>
Next year: <% tp.date.now("YYYY-MM-DD", "P1Y") %>

File's title date + 1 day (tomorrow): <% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>
File's title date - 1 day (yesterday): <% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>

Date tomorrow with format: <% tp.date.tomorrow("Do MMMM YYYY") %>    

This week's monday: <% tp.date.weekday("YYYY-MM-DD", 0) %>
Next monday: <% tp.date.weekday("YYYY-MM-DD", 7) %>
File's title monday: <% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD") %>
File's title next monday: <% tp.date.weekday("YYYY-MM-DD", 7, tp.file.title, "YYYY-MM-DD") %>

Date yesterday with format: <% tp.date.yesterday("Do MMMM YYYY") %>

```  

## date Module 

### Documentation 
#### tp.date.now 
**语法：**
```javascript
tp.date.now(format: string = "YYYY-MM-DD", offset?: number⎮string, reference?: string, reference_format?: string)
```
**参数：**

| 参数             | 说明                                    |
| ---------------- | --------------------------------------- |
| format           | 日期格式                                |
| offset           | 日期偏移量，例如设置`-7` 取上周的日志。 |
| reference        | 日期的引用，可以将日志作为笔记的标题 。 |
| reference_format | 日期的引用格式。                        | 

`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)
### tp.date.tomorrow
返回明天的日期。
**语法：**
```
tp.date.tomorrow(format: string = "YYYY-MM-DD")
```
**参数：**
| 参数             | 说明                                    |
| ---------------- | --------------------------------------- |
| format           | 日期格式                                |
`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)

### tp.date.weekday
返回星期几,是哪一个天。

```javascript
tp.date.weekday(format: string = "YYYY-MM-DD", weekday: number, reference?: string, reference_format?: string)
```
**参数：**

| 参数             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| format           | 日期格式                                                 |
| weekday          | 一周编号，星期一为第一天，编号为`0`，`-7` 上周的第一天。 |
| reference        | 日期的引用，可以将日志作为笔记的标题 。                  |
| reference_format | 日期的引用格式。                                         |
**示例**
```javascripts
 <% tp.date.weekday( "YYYY-MM-DD",0) %>  
```
![](https://www.aming.work:8084/images/2022/11/27/20221127152433.png)

## tp.date.yesterday

返回昨天的日期
**参数：**

| 参数             | 说明                                                     |
| ---------------- | -------------------------------------------------------- |
| format           | 日期格式                                                 |
`format`全部可以设置的格式：
[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/format/)



## Moment.js
`templater` 可以访问`moment` 对象。获取更多的函数。请参见[Moment.js | Docs (momentjs.com)](https://momentjs.com/docs/#/displaying/)
## Examples
```javascript
Date now: <% tp.date.now() %>
Date now with format: <% tp.date.now("Do MMMM YYYY") %>

Last week: <% tp.date.now("dddd Do MMMM YYYY", -7) %>
Today: <% tp.date.now("dddd Do MMMM YYYY, ddd") %>
Next week: <% tp.date.now("dddd Do MMMM YYYY", 7) %>

Last month: <% tp.date.now("YYYY-MM-DD", "P-1M") %>
Next year: <% tp.date.now("YYYY-MM-DD", "P1Y") %>

File's title date + 1 day (tomorrow): <% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>
File's title date - 1 day (yesterday): <% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>

Date tomorrow with format: <% tp.date.tomorrow("Do MMMM YYYY") %>    

This week's monday: <% tp.date.weekday("YYYY-MM-DD", 0) %>
Next monday: <% tp.date.weekday("YYYY-MM-DD", 7) %>
File's title monday: <% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD") %>
File's title next monday: <% tp.date.weekday("YYYY-MM-DD", 7, tp.file.title, "YYYY-MM-DD") %>

Date yesterday with format: <% tp.date.yesterday("Do MMMM YYYY") %>

```