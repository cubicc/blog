---
title: 使用PicGo+jsdelivr搭建Github图床
date: 2021-10-09 14:51:32
tags: 博客搭建 图床
---



​	最近在搭建博客时，为了美化界面和彰显个性，会使用自己的图片装饰博客。然而高清大图所需要的流量太大，消耗带宽资源过多。并且如果直接存储在Github Page这种境外服务器中，会影响到访问速度。所以了解到了图床服务。这个Blog使用PicGo工具、jsdelivr作为CDN搭建了Github图床。

## 图床是什么

图床 （Image hosting service），Wiki上是这么定义的：

​	An **image hosting service** allows individuals to upload [images](https://en.wikipedia.org/wiki/Image) to an [Internet](https://en.wikipedia.org/wiki/Internet) website. The image host will then store the image onto its server, and show the individual different types of code to allow others to view that image. Some of the best known examples are [Flickr](https://en.wikipedia.org/wiki/Flickr), [Imgur](https://en.wikipedia.org/wiki/Imgur) and [Photobucket](https://en.wikipedia.org/wiki/Photobucket), each catering for different purposes.

​	简单来说就是图床服务就是一个图片在线存储服务器，图片存储在他们的服务器CDN中，你可以通过很多方式访问到图片，这样可以提高个人网站的图片加载速度，节约流量。

​	国外常用的图床有Imgur、sm.ms。国外图床的最主要缺点是**国内访问速度不理想**，目前博客中的首页图片和头像是使用sm.ms，每次清除缓存后访问时能感受到图片加载肉眼可见地慢:( 。

​	国内常用的图床有七牛云、腾讯云、阿里云OSS和一众免费图床等。笔者的建议是**不要用免费图床**。免费图床看似很好，但一旦服务挂掉或开启防盗链，你所引用的所有图片都将永远消失或不能使用。七牛云和腾讯云这些图床服务会有一些免费空间，速度也很快，但最大的问题是**域名需要备案**。笔者的博客是hexo生成静态文件搭建在Github Page上的，属于境外服务器，无法进行备案，所以国内大型商业图床也就无法使用了。

## 为什么使用github + jsdeliver作为图床

​	众所周知，github作为全球最大的~~同性交友网站~~，还是有保障的。但最大的问题依旧是国内访问速度太慢。但我们可以使用jsdeliver，一个免费CDN网站加速访问。这样我们就可以将图片直接存储到github上，通过jsdeliver外链进行访问，实现我们的图床功能啦！



## PicGo工具安装设置

​	首先我们需要一个可以快速将图片上传到Github的工具，这里笔者使用[PicGo](https://github.com/Molunerfinn/PicGo) 进行图片上传。PicGo支持国内外多种图床的上传，包括七牛、腾讯云、又拍云、Github、SM.MS、阿里云OSS、Imgur等，还有开发者为其开发了许多第三方图床插件，详细的可以去PicGo的Github首页查看。

​	由于Github下载速度实在感人，这里提供2.3.0 64位版本的国内网盘下载地址：

百度云(提取码：cubi) : https://pan.baidu.com/s/1LC1SRfuiPDVQAFigxNjbmg 

阿里云：https://www.aliyundrive.com/s/kVS1Ak8hnmY

下载后点击下一步安装即可。打开后的界面如下：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/PicGo-Setup.png)



接下来进行Github图床的设置。

首先需要先在Github中新建一个repository，name可以随意取，**需要设置repo为Public**：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/PicGo-Github-New-Repo.png)

​	下面需要给PicGo访问仓库的权限。点击github右上角头像 -> Settings，来到个人设置界面。选择Developer settings -> Personal access tokens：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009160551.png)

​	点击上方的Generate new token，进行密码验证后，进入到如下界面：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009160751.png)

​	Note随意填写，Expiration选择过期时间，可以选择7、30、60、90天，也可以自定义过期时间。Github因为信息安全强烈**不建议选择无过期时间**。Select scopes只选择repo复选框即可。点击生成，

即可得到一段token：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009161223.png)

将token复制，回到PicGo中，选择Github图床：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009161336.png)



- 仓库名填写 【用户名】/【仓库名】
- 分支名选择main
- Token即为刚才复制的token
- 存储路径其实无所谓啦，填写 data/ 上传时图片会保存到仓库的data文件夹中
- 自定义域名： 需要填写 https://cdn.jsdelivr.net/gh/【用户名】/【仓库名】@【分支名】

点击确定后，即可完成设置。若你只使用Github作为图床的话，可以将其作为默认图床。



## 上传图片测试

​	接下来我们就可以试试上传图片啦！

​	PicGo支持拖拽，选择文件，甚至剪贴板和URL上传。上传后也支持Markdown、HTML、URL等多种格式的图片访问。笔者就选择下面这张图片来测试图片上传吧：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/b9c2c375af8e910a7c01a612645ded5b.jpg)

​	~~Dva真好看~~ 

​	我们只需要将本地的照片拖到PicGo对应的位置，PicGo便会告诉我们上传是否成功。上传后也可以在相册查看上传过的照片并且对上传过的照片进行管理：

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009162736.png)

​	访问Github对应的repo，可以看到我们的照片已经上传到仓库啦！

![](https://cdn.jsdelivr.net/gh/cubicc/cloudImg@main/data/QQ截图20211009162911.png)

​		



## 总结

​	本文使用PicGo工具与jsdelivr 在Github搭建了自己的图床。最后讨论一下这种方式的优缺点。

优点：

- 方便快捷
- 不限空间（应该）

缺点：

- 所有人都可以访问上传的图片（不要上传一些私人、敏感照片）
- 所有图片都上传到某个文件夹中，不易管理
- 虽然PicGo提供相册，但实测在相册删除照片并不会删除仓库中的照片



总的来说，此种方式比较适合个人写博客，搭网站。还有什么其他方便快捷、又能满足隐私要求的图床方式，欢迎在下方评论或与笔者联系哦~ 

