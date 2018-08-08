---
layout: post
title: 'Jekyll + Github = 简单搭建一个个人博客'
subtitle: ‘轻量化静态博客搭建指南’
date: 2018-08-07
categories: Jekyll+GitHub
cover: '../../../assets/img/Jekyll-header.jpeg'
tags: Jekyll Github Gitee Markdown HTML JavaScript
---

在我成功试水，搭建了自己的个人博客后，我体会到了 [Jekyll](https://www.jekyll.com.cn) 制作网站的轻便性。而 GitHub Pages 对 Jekyll 的支持，又省去了建站时服务器和域名的搭建。在查阅网上的资料时，大牛们都是先在本地做好 Ruby + Jekyll 的环境，调试完成后再上传至 GitHub 上。但是我在安装 Jekyll 这一步卡住了。无奈之下我寻找了一种更加“轻量化”的解决方法：**直接在 GitHub 上对模版进行编辑和调试**，这样我们就不需要在本地搭建环境。不过因为 GitHub 在国内的访问速度较慢，所以这篇教程**同时**介绍 GitHub 和 Gitee 的建站方法。

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

### 3.修改作者

等待施工。。。

### 4.注册 GitHub/Gitee 账号

接下来我们应该注册账号，如果已经注册过了就可以跳过这一节。目前对 Jekyll Pages 的支持比较好的有 GitHub 和 Gitee。推荐先使用 GitHub ，再使用 Gitee 。（因为 Gitee 可以直接将 GitHub 的仓库同步过去）

[GitHub](https://github.com)

[Gitee](https://gitee.com)

### 5.创建个人站点的仓库

等待施工。。。




