---
{"author":"aming","email":"jikcheng@163.com","title":"obsidian Figure bed","creation_date":"2022-10-08 16:13","Last modified date":"2022-11-27 18:49","tags":"obsidian Figure bed","File Folder with relative path":"soft/Doc/obsidian/Plugin","remark":null,"other":null,"dg-publish":true,"permalink":"/soft/doc/obsidian/plugin/obsidian-figure-bed/","dgPassFrontmatter":true}
---



## 对于图床安装详情请见
[[NAS/Doc/Chevereto/Chevereto 图床安装\|Chevereto 图床安装]]
## obsidian 和Chevereto 结合
### 下载相关安装包
1. 下载PicGo
[Releases · Molunerfinn/PicGo (github.com)](https://github.com/Molunerfinn/PicGo/releases)
```ad-warning
必须使用正式版2.3.0 ,否则网络不稳定。 无法上传图片。

```
### 修改默认上传图片的位置
## 修改 Chevereto 上传用户

Chevereta 的 API 上传会把图片放在一个很特别的文件夹里，上传的相册ID为1（相册不可见），上传的用户名为 Guest。  
**以下教程是将图片上传到指定用户指定相册下，如果不需要可以跳过这段教程。**

### 获取相册 ID

点击用户名，在 `My Profile` 里找到 `Create new album`，**输入名字创建新相册**  
![图片[4]-Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程](https://tc.mspace.cc/images/2022/06/02/202206021119837.png)

进入刚刚创建的相册，**点击`Full info`，找到相册的 ID**  
![图片[5]-Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程](https://tc.mspace.cc/images/2022/06/02/202206021121975.png)

### 修改配置

进入宝塔，访问图床文件夹，找到如图所示的文件位置，**复制 `route.api.php` ，复制，不是剪切！**  
![图片[6]-Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程](https://tc.mspace.cc/images/2022/06/02/202206021124063.png)

粘贴到 `routes` - `overrides` 里，**双击打开** `route.api.php`  
![图片[7]-Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程](https://tc.mspace.cc/images/2022/06/02/202206021127716.png)

搜索 `uploaded_id =` ，将这一行的代码替换成以下内容（**用户名和相册ID修改成自己的**）

```
$uploaded_id = CHV\Image::uploadToWebsite($source, '用户名', array('album_id'=>相册ID));
```

![图片[8]-Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程](https://tc.mspace.cc/images/2022/06/02/202206021131515.png)

**如图所示，保存，API 上传的图片就会到指定用户的指定文件夹里了。**
2. 下载node.js
[下载 | Node.js 中文网 (nodejs.cn)](http://nodejs.cn/download/)
3. 配置PicGo 插件
![](https://www.aming.work:8084/images/2022/10/08/20221008183549.png)
4. 配置cheverto uploader 插件
![](https://www.aming.work:8084/images/2022/10/08/20221008183615.png)

```bash
Url: https://www.aming.work:8084/api/1/upload
KEY: f9f5c7a0e8b194d92bff304221da9399
```

![](https://www.aming.work:8084/images/2022/10/08/20221008203623.png)

## 配置obsidina 插件
1. obsidian-image-auto-upload-plugin
   [obsidian-image-auto-upload-plugin](https://github.com/renmu123/obsidian-image-auto-upload-plugin)
2. 设置插件和picgo 联动
![](https://www.aming.work:8084/images/2022/10/08/20221008183909.png)
```
http://127.0.0.1:36677/upload
```

到此obsidian 图床搭建完毕，尽情享受把。


## 参考资料
[PicGo | PicGo](https://picgo.github.io/PicGo-Doc/zh/guide/#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85)
[Chevereto 搭配 PicGo 自动上传图片+修改上传位置教程 (mspace.cc)](https://www.mspace.cc/archives/607)
[使用PicGo插件上传到自架Chereveto图床 - 邮莓生活 (mailberry.com.cn)](https://mailberry.com.cn/2019/12/picgo-upload-img-to-chereveto/)

[Typora + PicGo 自动上传图片到 Chevereto 图床 - 张天宇的博客 | ztygalaxy's Blog (tyzhang.top)](https://tyzhang.top/article/autoupload/)
