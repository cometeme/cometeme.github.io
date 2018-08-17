---
layout: post
title: '[Jekyll] permalink'
subtitle: 让文章的链接地址变得更整齐美观
date: 2018-08-12
categories: Jekyll
cover: '../../../assets/img/permalink-header.jpg'
tags: Jekyll Markdown HTML
---

访问这篇文章时，你有注意到地址栏中显示的地址吗？它应该是这样的： `https://cometeme.github.io/jekyll/2018/08/Jekyll-为文章设置永久链接.html` 。打开其他的文章，可以看到我给每篇文章都配置了类似的链接地址。这样的链接形式比起单纯的文章名要更整齐美观。当你在使用 Jekyll 的模版时，一般就已经预设了一种链接形式。不过如果你想更改这种链接形式，就可以参考下以下的教程：

### 1. permalink 参数的修改位置

在 Jekyll 的架构下，我们只需要打开 `_config.yml` 这个文件，就可以在里面找到 `permalink` 这个参数了。在不同的模版下，初始的形式各不相同。

> 如果 `_config.yml` 中没有 `permalink` 参数，那代表它使用了默认的参数。如果你不希望使用默认的，可以在文件末尾加上这个参数。

### 2. permalink 的常见参数

permalink 使用 `:` 来标记关键词。其中有以下这一些关键词：

-   year
    年份

-   month
    月份

-   i_month
    短月份(不带开头的0)

-   day
    日期

-   i_day
    短日期(不带开头的0)

-   title
    文章标题

-   categories
    文章目录，如果没有目录，会自动忽略

所以要实现 `/jekyll/2018/08/Jekyll-为文章设置永久链接.html` 这样的效果，我们只需要配制成

```html
permalink: /:categories/:year/:month/:title.html
```

这样的格式就可以了。

如果要在每一级的目录内添加多个参数也是可以的，比如 `/:year-:month-:day/` 最后显示的结果就是 `/2018-08-12/`

### 3. permalink 的预置参数

其实， permalink 还带有三个预置好的参数。其中默认的参数就是 date 。它的三个参数如下：

-   date
    `/:categories/:year/:month/:day/:title.html`

-   pretty
    `/:categories/:year/:month/:day/:title/`

-   none
    `/:categories/:title.html`

date 和 none 的参数都很好理解，但是 pretty 这个参数结尾的形式就很好玩了。它提醒我们的是：如果最后为 `/:title/` 而非 `/:title.html` 的话，显示出来的网页地址就不会带 `.html` 这个后缀，这样更美观了。所以当你在创建时，也可以通过这样的设置来实现不带后缀的链接地址。

### 结语与其他文档

permalink 参数可以让我们更加灵活地改变 Jekyll 网页中文章的地址。其实 `_config.yml` 这个文件中还有许多的参数可以供我们调整，以此实现更加自定义化的网页。希望大家在掌握了 permalink 的调整方法之后，也能自己去学习其他参数的作用，从而让自己的网站更加完美。
