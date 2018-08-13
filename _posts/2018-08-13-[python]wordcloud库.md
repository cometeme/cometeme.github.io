---
layout: post
title: '[python] wordcloud 库'
subtitle: '将一串文本转化为词云'
date: 2018-08-13
categories: python第三方库
cover: '../../../assets/img/wordcloud-header.jpg'
tags: python python库 python第三方库
---

了解 python 时，别人会告诉你 python 的一个十分常见的作用就是“大数据分析”。不过接触了 python 的基础语法之后，你可能觉得它好像与“数据处理”没有太多的关系。的确， python 基础库的功能较为有限，不过 python 的强大之处其实在于它丰富多彩的第三方库。在 [PyPI](https://pypi.org) 这个平台上，你可以看到绝大多数第三方库的资料。而今天我们的主角是 wordcloud 库。

### 1.wordcloud 库的安装

不同于其他编程语言， python 安装第三方库的方法特别简单，只需要用 `pip` 指令就可以很轻松地完成了：

```python
  pip install wordcloud
```

>如果提示权限不足，可以使用管理员或是 `sudo` 指令

>如果遇到安装缓慢，或者是连接失败的问题，可以尝试切换为国内源。

### 2.生成一个简单的词云

首先我们先随便找一段**英文文本**，你可以将它放在字符串中，也可以存放在文件里。这里我们就以文件为例。

wordcloud 库中有三个常用的函数： `WordCloud()` 、 `generate()` 、 `to_file()` 。它们的功能分别如下：

* WordCloud() 生成一个 wordcloud 对象

* generate(string) 从文本中生成结果

* to_file(fileName) 导出到文件（最好是.png格式）

这三个函数的使用顺序是固定的，先定义对象，再生成结果，最后进行导出。

介绍了上面这三个函数之后，我们先用一段代码来演示一下（ in.txt 中的文本可以自己找，但是必须是纯英文的）：

```python
  from wordcloud import WordCloud

  intxtname = 'in.txt'
  outpngname = 'wordcloud.png'

  infile = open(intxtname, 'r', encoding="utf-8")
  intxt = infile.read()

  wc = WordCloud()

  wc.generate(intxt)
  wc.to_file(outpngname)
  infile.close()
```

运行完成后，可以看到生成了这样一张图片：

![WordCloudSimple](../../../assets/screenshot/wordcloud-1.png)

怎么样，是不是很神奇。只需要简单的三个函数，我们就能实现一个词云的输出。当然了，如果单纯地使用默认参数，那未免也太单调了。所以我们还需要进行自定义。

### 3.自定义词云

* width
指定词云对象生成图片的宽度，默认400像素

* height
指定词云对象生成图片的高度，默认200像素

* min_font_size
指定词云中字体的最小字号，默认4号

* max_font_size
指定词云中字体的最大字号，根据高度自动调节

* font_step
指定词云中字体字号的步进间隔，默认为1

* font_path
指定字体文件的路径，默认None

* max_words
指定词云显示的最大单词数量，默认200

* stop_words
指定词云的排除词列表，即不显示的单词列表

* background_color
指定词云图片的背景颜色，默认为黑色
`wc=wordcloud.WordCloud(background_color = “white”)`


待施工
