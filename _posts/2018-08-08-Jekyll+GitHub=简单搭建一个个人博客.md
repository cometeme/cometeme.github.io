---
layout: post
title: 'Jekyll + Github = 简单搭建一个个人博客'
subtitle: ‘轻量化静态博客搭建指南’
date: 2018-08-07
categories: Jekyll+GitHub
cover: '../../../assets/img/Jekyll-header.jpeg'
tags: Jekyll Github Gitee Markdown HTML JavaScript
---

在我成功试水，搭建了自己的个人博客后，我体会到了 [Jekyll](https://www.jekyll.com.cn) 制作网站的轻便性。而 [GitHub Pages](https://pages.github.com) 对 Jekyll 的支持，又省去了建站时服务器和域名的搭建。在查阅网上的资料时，大牛们都是先在本地做好 Ruby + Jekyll 的环境，调试完成后再上传至 GitHub 上。但是我在安装 Jekyll 这一步卡住了。无奈之下我寻找了一种更加“轻量化”的解决方法：**直接在 GitHub 上对模版进行编辑和调试**，这样我们就不需要在本地搭建环境。不过因为 GitHub 在国内的访问速度较慢，所以这篇教程**同时**介绍 GitHub 和 Gitee 的建站方法。

### 1.寻找模版

在开始之前，我们需要先到 [Jekyll Themes](http://jekyllthemes.org) 上面寻找自己喜欢的模版。模版介绍界面的 `Demo` 按钮可以查看模版的示例网站。如果你喜欢这个模版，你可以点 `Download` 将其下载并解压。

### 2.简单介绍文件结构

解压完成之后，你应该能看到这样一些文件夹。以我的网站为例

![Files](../../../assets/screenshot/Jekyll-1.png)

不同的模版可能文件的个数不一样，但是一定会有这几个关键文件：

* index.html 首页的文件

* _config.yml 设置你的网站信息

* 404.html 自定义404界面（这个一般都会有，如果没有的话就会使用最“原始”的404页面）

* README.md （很重要！）作者一般会在这个文件中介绍如何配置模版。如果没有这个文件，请重新打开你下载的模版的介绍页面，下方应该会有提示。

同时，文件目录下应该也会有这些文件夹：

* _post 存放你的文章

* _layouts 网页模版文件

* _includes 导航栏，页脚等组件模版

### 3.修改信息

接下来就是对你的网站进行自定义的环节。

先打开 `README.md` 这个文件并仔细阅读。打开 `_config.yml` ，根据里面有的注释进行修改。因为没有搭建本地环境。所以修改后的成果是无法查看的。当然 `_config.yml` 在之后都可以随时修改，所以在稍微修改之后，我们就可以先进行下一步。等到网站搭建完成了之后可以继续细调。

![_config.yml](../../../assets/screenshot/Jekyll-2.png)

### 4.注册 GitHub/Gitee 账号

接下来我们应该注册账号，如果已经注册过了就可以跳过这一节。目前对 Jekyll Pages 的支持比较好的有 GitHub 和 Gitee。推荐先使用 GitHub ，再使用 Gitee 。（因为 Gitee 可以直接将 GitHub 的仓库同步过去）

网址：

* [GitHub](https://github.com)

* [Gitee](https://gitee.com)

### 5.创建个人站点的仓库

现在我们可以创建一个仓库（ Repository ）。不过 Pages 分为**项目 Pages** 和**个人 Pages** 。其中**个人 Pages** 可以通过特殊的域名访问。（比如本站是 `https://cometeme.github.io` ）要创建个人 Pages ，仓库的名称必须要符合一定的规则。 GitHub 与 Gitee 两者的名称规则不同。

* GitHub：仓库名称要求为 `username.github.io`
 
* Gitee：仓库名称要求为 `username`
 
> 如果想要之后直接将 GitHub 上面的项目克隆至 Gitee ，那么暂时先不要创建 Gitee 的项目。具体如何克隆我会在之后发布新的文章介绍。

项目创建完成后，我们打开项目，将本地的修改过的模版文件框选拖至窗口中，等待上传完成后提交。（底下的介绍可以用默认的，建议直接提交至 master 分支）。提交完成后，就是以下的效果了：

![site preview](../../../assets/screenshot/Jekyll-3.png)

### 6.开启 Pages 服务

* GitHub

选中菜单的 `Settings` 选单，找到下方的 `GitHub Pages` 选项。（本图中已经开启，如果没有开启只需要点击相应的按钮开启即可）其中上方提示 "Your site is published at https://cometeme.github.io/" 说明已经部署完成了。这个时候你点击那个链接就可以看到自己的网页。（ GitHub 的页面更新速度较慢，有些时候做一定的更改要过段时间刷新才生效）

![settings menu](../../../assets/screenshot/Jekyll-4.png)

![GitHub Pages settings](../../../assets/screenshot/Jekyll-5.png)

* Gitee

选择菜单中的 `服务` ，进入 `码云 Pages` 页面。并在下方选中 master 分支，点击部署。部署完成后同样会给出一个链接。点开就是架好的博客了。

![服务-码云 Pages ](../../../assets/screenshot/Jekyll-6.png)

![码云  Pages ](../../../assets/screenshot/Jekyll-7.png)

如果到这里你的网页能够成功显示，那么恭喜你，你完成了建站所需要的所有步骤。接下来就只需要对界面再进行一些微调（比如修改首页图片，自定义 `about` 页面等），并发布你的文章。

### 7.修改首页图片/个人头像

如果单纯地套用模版，大家做出来的网页就难免会“撞脸”。这个时候，自定义一下首页的图片是最好的选择。

随便翻翻模版的文件，总能够找到几张图片格式的文件。它们其中就有首页的头图。当你找到它之后，你只需要用一张新的图片替换就可以简便地更换首页的图片。 不过永远要注意的是，上传图片前最好先进行一定的裁剪使得图片适合那块区域的大小。其次最好对图片进行一定的压缩，一般 `800kb` 是一个比较好的选择。

寻找图片是不是很头痛？给大家推荐一个好用的“**免费无版权**“的图库： [Unsplash](https://unsplash.com) 。它上面的图片可以直接用于商用而不需要任何版权授权，而且 Unsplash 上的图片品质都比较高，我博客上的所有图片都是来源于它。

找到一张合适的图片并进行处理后，将它命名为与原头图名称相同的文件（包括文件格式），然后上传到 GitHub 上面就可以将原来的图片覆盖，实现替换。不过为了避免出错，最好先将原来的图片下载下来“备份”一下。同样的，只要你能找得到网页的各部分图片，你都能将它们替换。

一般更改完图片，或是发布一篇新的文章后，网页需要较长的时间来更新。（也就是说有可能你刷新几遍发现图片没换）这时千万不要以为，等待1-2分钟之后再刷新试试）

### 8.什么是 Jekyll YAML？

待施工。。。









