---
layout: post
title: '为 Github Pages 绑定域名'
subtitle: '使用自己的域名来访问博客'
date: 2018-08-26
categories: Web
cover: '/assets/banner/custom-domain-header.jpg'
tags: Web Github
---

之前，我在 Github 上建了个个人博客，不过我希望还是能使用自己的域名。现在 Github 已经支持让自定义的域名使用 https 加密了，所以我决定更换一下自己的域名。

### 1. 修改 DNS 解析的数据

在设置 Github 之前，我们最好先修改 DNS 解析的数据。因为如果步骤颠倒的话， Github Pages 就不能开启 https ，必须要重新设置一遍才行。所以我们先打开域名的 DNS 解析控制台。添加一条 CNAME 设置，主机记录设置为 www ，记录值设置为自己博客的站点（ xxx.github.io ）不能包含 www 或 https 前缀。

光有 CNAME 这项设置并不能完成解析，我们还需要添加一个 A 设置，来指向 Github 的服务器。

翻阅 Github 上关于自定义域名的介绍，只需要将一个 A 设置指向下面其中的一个 ip 地址，就能开启 https 加密了。当然， A 的主机记录需要设置为 @ 。

> If you configured your custom domain using an A record, your A record must point to one of the following IP addresses for HTTPS to work:
>
> -   185.199.108.153
> -   185.199.109.153
> -   185.199.110.153
> -   185.199.111.153

所以，有强迫症的我最终把上面的四个 ip 都设置上去了，最终的效果就是如下的：

![DNS sample](/assets/image/custom-domain-1.png)

这样我们就完成了 DNS 解析的设置了，接下来我们要修改 Github 上的设置：

### 2. 为 Github Page 设置 CNAME

打开你要绑定域名的项目，进入设置页面。并在 Github Pages - Custom domain 这一栏填上你的网址。

![Github Pages setting](/assets/image/custom-domain-2.png)

注意，一定要填写带 www 的网址！如果你设置了不带 www 的网址，那么如果访问 `www.xxx.com` 就会无连接。

> If your domain has HTTPS enforcement enabled, GitHub Pages' servers will not automatically route redirects. You must configure www subdomain and root domain redirects with your domain registrar.

设置完成后，我们刷新一下，如果底下的 Enforce HTTPS 选项已经可以勾选，那么我们钩上它。

大约需要等 2-10 分钟，之后我们打开 xxx.github.io 的网页，应该就会自动跳转到自己的域名。并且能够显示出一个小绿锁。这说明这个网页使用的已经是 https 加密协议了。

![complete!](/assets/image/custom-domain-3.png)

### 结语与其他文档

这样，我们就给 Github Pages 绑定自己的域名了！不得不说， Github Pages 除了有些慢之外，它的设置可比自建服务器简单多了。如果你还想更进一步自定义自己的域名的话，可以参考它的官方文档： [Using a custom domain with GitHub Pages](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) 。
