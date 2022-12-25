---
{"author":"aming","email":"jikcheng@163.com","title":"Templater Syntax","creation_date":"2022-11-27 13:03","Last modified date":"2022-11-27 18:48","tags":"Templater Syntax","File Folder with relative path":"soft/Doc/obsidian/Plugin/Templater","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/templater/templater-syntax/","dgPassFrontmatter":true}
---


`Templater` 使用一个自定义模板引擎（rusty_engine) 语法声明一个命令。你需要花一点时间去使用和掌握他。一旦使用了一次后，这种语法并不难掌握。

`Templater` 所有的函数都是使用命令调用`JavaScript` 对象。

## 命令语法（command syntax）
命令开头必须使用`<%` 和结束使用 `%> ，构成一个完整的命令。

这里有一个完整的命令使用案例：`<% tp.date.now() %>` 

## 函数语法（Function syntax）
### 函数对象层次结构
模板中所有的函数，包括内部函数和用户函数，都可以在`tp` 对象下进行调用。可以说所有函数是`tp` 对象的子级，要访问对象的子级，需要使用`.` 符号。

换句话说，如果需要调用函数则必须使用`tp.<somethins>` 这种调用方式。

### 函数调用
调用某个函数，需要使用特定的函数调用语法：在函数名称前添加`(` ，函数名称后添加`)`。


例如：可以使用`tp.date.now()` 调用`tp.date.noew` 内部函数。


函数可以传入参数，参数分为`可选` ,`必选`。如下所示：
```
tp.date.now(arg1_value, arg2_value, arg3_value, ...)
```

> [!warning]
> 所有参数传入顺序要正确。

参数拥有不同的类型，以下列出了常见的参数类型。

- `string` (字符串类型)，要使用单引号或者双引号，像这样`"values"` 或者`'values'` 。
- `number`（数字类型），目前必须使用整数类型（15，-5，...)
- `boolean`(布尔类型)，可以是`true` 和`false` ，（必须小写）。

调用函数时需要指定参数的类型，否则函数调用不起作用。

## 文档中函数语法说明

文档中所有的函数都使用以下语法：
```javascript
tp.<my_function>(arg1_name: type, arg2_name?: type, arg3_name: type = <default_value>, arg4_name: type1|type2, ...)
```

| 参数                              | 说明                                  |
| --------------------------------- | ------------------------------------- |
| arg_name                          | 参数名称                              |
| type                              | 参数的数据类型                        |
| arg2_name?: type                  | 如果参数是可选的，他将附加一个问好`?` |
| arg3_name: type = <default_value> | 如果参数有默认值，会附加一个等号。    |
| arg4_name: type1\|type2           | 如果一个参数有不同的数据类型。        | 

### 语法警告
此语法仅用于文档目的，以便能够理解函数所需参数。

调用函数时，不得指定参数的名称、类型和默认值。只有参数的值是必须的，

**Example**
我们以 `tp.date.now() 内部函数 为例：
```javascript
tp.date.now(format: string = "YYYY-MM-DD", offset?: number|string, reference?: string, reference_format?: string)
```

这个内部函数有4 个可选参数：
- `format` 形参需要传入`string` 类型，默认值为"YYYY-MM-DD"。
- `offset` 形参需要传入`nmber`或者`string` 。
- `reference` 形参需要传入`string` 类型。
- `reference_format` 形参需要传入`string`类型。
从以上语法得出此参数的有效调用方法为以下几种：
-   `<% tp.date.now() %>`
-   `<% tp.date.now("YYYY-MM-DD", 7) %>`
-   `<% tp.date.now("YYYY-MM-DD", 7, "2021-04-09", "YYYY-MM-DD") %>`
-   `<% tp.date.now("dddd, MMMM Do YYYY", 0, tp.file.title, "YYYY-MM-DD")%>` *Assuming the file name is of the format: "YYYY-MM-DD"

无效的调用：
-   `tp.date.now(format: string = "YYYY-MM-DD")`
-   `tp.date.now(format: string = "YYYY-MM-DD", offset?: 0)`