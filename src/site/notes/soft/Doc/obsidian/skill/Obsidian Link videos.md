---
{"author":"aming","email":"jikcheng@163.com","title":"Obsidian Link videos","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-27 19:42","tags":"Obsidian Link videos","File Folder with relative path":"soft/Doc/obsidian/skill","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/skill/obsidian-link-videos/","dgPassFrontmatter":true}
---






>完整视屏
[Johnny学OB 第18集 - 用OB给音频和视频加时间戳，你想看哪儿，就去看哪儿，更加高效的学习。Media Extended详细教程 Obsidian教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1hf4y1P7iN?spm_id_from=333.999.0.0)

# 如何使用obsidian 管理视屏
# 嵌入库内
直接拖拽进入到对应的文件夹里面。内容可以是音频或者视屏
[01:58](https://www.bilibili.com/video/BV1hf4y1P7iN?spm_id_from=333.999.0.0#t=118.020175)
> [!question]
>1、 会造成obsidian 库比较大
> 2、会造成连接丢失。文件地址变了，会造成链接失效。


# 嵌入库外

## 嵌入哔哩哔哩
[02:55](https://www.bilibili.com/video/BV1hf4y1P7iN?spm_id_from=333.999.0.0#t=175.859923)
> [!note]
> 只要网站在 ，永远都有效。

## 嵌入NAS视屏
> [!error]
> obsidian 的extend 插件 无法使用SMB 协议，所以只好，改为WEBDEV 协议。
>  参考[[NAS/Doc/webdev/群辉开启webDEV并配置\|群辉开启webDEV并配置]]
>  2、注意NAS相关的视屏目录需要精心设计，否则，以后很难管理。

1、 拿到文件路径
```
L:\[视频合集]kingbase\冷健全--共论行业发展之道解生态建设之惑.mp4
```
2、windows 使用total commander 使用`CTRL+3`

3、编写连接
```
[冷健全--共论行业发展之道解生态建设之惑](file:///L:\[视频合集]kingbase\冷健全--共论行业发展之道解生态建设之惑.mp4)
```

[冷健全--共论行业发展之道解生态建设之惑](file:///L:\[视频合集]kingbase\冷健全--共论行业发展之道解生态建设之惑.mp4)
4、在预览模式下CTL+鼠标左键点击，就可以关联相关视屏进行标注和学习。


