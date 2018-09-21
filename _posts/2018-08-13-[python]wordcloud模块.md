---
layout: post
title: '[python] wordcloud 模块'
subtitle: '将一串文本转化为词云'
date: 2018-08-13
categories: python
cover: '../../../assets/img/wordcloud-header.jpg'
tags: python python模块 python第三方模块
---

了解 python 时，别人会告诉你 python 的一个十分常见的作用就是“大数据分析”。不过接触了 python 的基础语法之后，你可能觉得它好像与“数据处理”没有太多的关系。的确， python 基础模块的功能较为有限，不过 python 的强大之处其实在于它丰富多彩的第三方模块。在 [PyPI](https://pypi.org) 这个平台上，你可以看到绝大多数第三方模块的资料。而今天我们的主角是 wordcloud 模块。

### 1. wordcloud 模块的安装

不同于其他编程语言， python 安装第三方模块的方法特别简单，只需要用 `pip` 指令就可以很轻松地完成了：

```bash
$ pip install wordcloud
```

> 如果提示权限不足，可以使用管理员或是 `sudo` 指令
>
> 如果遇到安装缓慢，或者是连接失败的问题，可以尝试切换为国内源。

### 2. 生成一个简单的词云

首先我们先随便找一段**英文文本**，你可以将它放在字符串中，也可以存放在文件里。这里我们就以文件为例。

wordcloud 模块中有三个常用的函数： `WordCloud()` 、 `generate()` 、 `to_file()` 。它们的功能分别如下：

-   WordCloud() 生成一个 wordcloud 对象

-   generate(string) 从文本中生成结果

-   to_file(fileName) 导出到文件（最好是.png格式）

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

![WordCloudSimpleBlack](../../../assets/screenshot/wordcloud-1.png)

怎么样，是不是很神奇。只需要简单的三个函数，我们就能实现一个词云的输出。当然了，如果单纯地使用默认参数，那未免也太单调了。所以我们还需要进行自定义。

### 3. 自定义词云

在初始化 wordcloud 对象时，我们使用的是 `WordCloud()` 函数。之前的设置里面我们没有在其中加入任何参数，不过其实 `WordCloud()` 函数可是有许多参数的：

-   width
    指定词云对象生成图片的宽度，默认400像素

-   height
    指定词云对象生成图片的高度，默认200像素

-   min_font_size
    指定词云中字体的最小字号，默认为4

-   max_font_size
    指定词云中字体的最大字号，根据高度自动调节

-   font_step
    指定词云中字体字号的相差间隔，默认为1

-   font_path
    指定字体文件的路径，默认None（可以将字体文件放在统一目录下引用）

-   max_words
    指定词云显示的最大单词数量，默认200

-   background_color
    指定词云图片的背景颜色，默认为黑色，若要指定为白色，可设置为：
    `wc=wordcloud.WordCloud(background_color = “white”)`

-   mask
    设定词云的遮罩（限定词云的形状）

所以，让我们试试将输出的图片改大一些。背景换为白色。

```python
wc = WordCloud(height=600, width=800, background_color='white')
```

我们应该会得到如下的图片：

![WordCloudSimpleWhite](../../../assets/screenshot/wordcloud-2.png)

是不是看起来比上面的黑色图片好多了。可能你会发现这个词云中没有 200 个单词。其实这是因为我挑的文本过短，而且 wordcloud 本身会过滤一些较短的单词，所以导致最后输出的结果较少了。

那么还有个 `mask` 属性没有讲，因为它比上面这些稍微复杂一些，所以我们单独整理：

### 4. 为词云设置 mask 属性

为词云设置 `mask` 属性时，我们需要用到 scipy.misc 模块中的 `imread` 这个函数。它是一个图像操作的模块，在这里我们只需要知道它可以读取一个图像就行了。

让我们看一下下面这样一个代码：

```python
from wordcloud import WordCloud
from scipy.misc import imread
mask = imread(“WWDC18.png”)

intxtname = 'in.txt'
outpngname = 'WWDC18wordcloud.png'

infile = open(intxtname, 'r', encoding="utf-8")
intxt = infile.read()

wc = WordCloud(height=600, width=800, background_color='white', mask=mask)

wc.generate(intxt)
wc.to_file(outpngname)
infile.close()
```

可以看到，我们需要先导入 `imread`，然后用 `imread` 读取一个图片文件。之后我们将读取到的文件设置为 wordcloud 对象的 `mask` 属性。那这样可以实现怎么样的一个效果呢？

![WWDC18.png](../../../assets/screenshot/wordcloud-3.png)

它可以读入上面这样一个图片，最后输出以下的图案：

![WWDC18wordclou.png](../../../assets/screenshot/wordcloud-4.png)

只需要多加几步，便可以实现遮罩的效果了。 wordcloud 这个模块的一个特点就是简单易用。

### 结语与其他文档

这样我们就完成了 wordcloud 的简单介绍了。当然，它还有许多更高级的用法，比如让生成的词云颜色符合原图的颜色，给遮罩描边等功能，都可以在它的 GitHub 示例中找到： [A little word cloud generator in Python](https://github.com/amueller/word_cloud)
