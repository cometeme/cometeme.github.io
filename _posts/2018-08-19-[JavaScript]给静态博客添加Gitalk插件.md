---
layout: post
title: '[JavaScript] 给静态博客博客添加 Gitalk 插件'
subtitle: '基于 Github Issues 的评论解决方案'
date: 2018-08-19
categories: JavaScript
cover: '/assets/img/Gitalk-header.jpg'
tags: Jekyll Github Javascript CSS HTML
---

作为一个静态博客， Jekyll 并没有自带评论系统，但是有了评论模块可以让大家更方便地交流意见，灌灌水之类的，所以我也一直想尝试为自己的博客添加评论插件。

一开始查找评论插件时，发现大家主要在用的是 Disqus ，但是 Disqus 现在在国内无法使用了，很多情况下它都无法正常加载。而本来可以用的多说在 2017 年停止了服务。后来我发现了 [Gitment](https://github.com/imsun/gitment) ，但是它也很久没有维护了。最终我找到了一个叫 Gitalk 的评论插件。它的原理和 Gitment 类似，它通过在 GitHub 仓库中的 issues 中添加回答来实现评论的功能。这是它的仓库链接：[Gitalk on Github](https://github.com/gitalk/gitalk)

### 1. 在前端页面中引入 Gitalk

在使用之前，先要在页面的适当位置插入一个 Gitalk 的 div 容器：

```html
<div id='gitalk-container'></div>
```

在页面文件最前面可以通过链接添加 Gitalk 的 css 文件与 js 支持：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
<script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>

<!-- or -->

<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```

> 如果出现 gitalk 的 css 文件将原来的 markdown 样式覆盖的情况，可以将 css 文件下载到本地，并删去 markdown 的样式，之后引用本地的 css 文件就可以解决问题。

### 2. 添加 Javascrpt 块

我们可以通过引用 js 文件或者直接添加 script 标签来设置 Gitalk ，这里我使用直接添加 script 标签的方式：

```html
<script>
const gitalk = new Gitalk({
  clientID: 'GitHub Application Client ID',
  clientSecret: 'GitHub Application Client Secret',
  repo: 'GitHub repo',
  owner: 'GitHub repo owner',
  admin: ['GitHub repo owner and collaborators, only these guys can initialize github issues'],
  id: location.pathname,      // Ensure uniqueness and length less than 50
  distractionFreeMode: false  // Facebook-like distraction free mode
})

gitalk.render('gitalk-container')
</script>
```

我们首先需要填写的是 `repo` ， `owner` ， `admin` 这三个参数：

-   repo
    仓库名称

-   owner
    仓库所有者

-   admin
    可以创建 Github issues 的管理者

但是其中， `id` 这一个也需要我们修改，因为 `id` 的默认值为路径，而它**不能超过50个字符**，不然会出现 `Error: Validation Failed.` 这个错误。在 Jekyll 的框架下，我们可以将这个值改为 `'{{ page.title }}'` ，这样一般来说就不会超过长度了。

Gitalk 对象还有许多参数，其中较为常用的还有以下两个：

-   createIssueManually
    如果为 true ，则在不存在当前页面的 issue 的情况下可以由管理员手动创建，推荐打开，默认为 false

-   title
    设置 issue 的标题，如果使用 Jekyll 推荐改为 `'{{ page.title }}'` ，不然默认为网页标题（可能会很长）

其他的参数可以参考 Gitalk 的文档。

### 3. 创建一个 GitHub Application

在 Javascript 代码中，还有 `clientID` 和 `clientSecret` 这两个属性没有填写。它们就和 GitHub Application 有关了。

首先我们需要注册一个GitHub Application：

[Register a new OAuth application](https://github.com/settings/applications/new)

其中的 `Application name` 可以随便填， `Homepage URL` 和 `Authorization callback URL` 需要填写你**博客网页**的网址。

生成一个 GitHub Application 之后，网页会显示一个 clientID 和一个 clientSecret ，将其复制到代码中。其中这两个参数**只会显示一次**，所以一定记得填写完之后要保存代码。

### 4. 连接你的 GitHub 账号并新建 issue

以上我们就完成了代码内容，现在我们生成静态页面，打开一篇文章，应该就可以在相应的区域看到 Gitalk 的插件了。第一次使用时，我们需要“**初始化issue**”，这个时候你需要用你的 Github 账号登陆，然后点击一下初始化按钮（不要点很多次，不然会新建多个issue），等进度完成后，刷新页面，你应该就能够看到评论模块了。

![Gitalk-sample](/assets/screenshot/Gitalk-1.png)

目前来说我们只能通过手动添加来给每篇文章创建一个评论，之后可以写一个脚本来完成自动初始化，但是稍有些复杂，所以如果想要了解可以自行搜索。

### 结语与其他文档

以上就完成了 Gitalk 插件的安装了，这样也能给博客增加些活跃的气氛。不过希望之后能够出现更加方便的评论插件。（ Gitalk 需要登陆 GitHub 才能评论）如果大家有更好的插件也可以留言分享。
