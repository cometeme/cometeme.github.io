---
layout: post
title: '[vscode] Markdown Preview Enhanced'
subtitle: '强大好用的内置代码补全插件'
date: 2019-06-29
categories: vscode
cover: '/assets/image/mpe-header.png'
tags: vscode 插件
---

在日常的写作中， Markdown 已经成为了我的首选工具。其简洁的语法，舒适的写作体验，以及美观的渲染结果，给我带来了很好的体验。然而，由于 Markdown 过于简洁，它本身支持的功能也比较少。在平时的记录中，我可能会需要在文档中插入一些公式或者符号，这时单纯地使用 Markdown 就无法实现了。

不过，说到公式与符号，我们就很容易想到 Markdown 的一个老朋友 —— LaTeX 。在排版方面， LaTeX 还是非常强大的，相信大家如果有写过学术论文的经历的话应该都会认识它。

那么，有没有能够将 LaTeX 中的公式功能引入到 Markdown 语法中的方法呢？当然有！通过 [KaTeX](https://github.com/Khan/KaTeX) 和 [MathJax](https://github.com/mathjax/MathJax) 这个两个插件，我们就能够在 Markdown 中插入公式，并且通过它们将公式渲染出来。

在 vscode 中，我找到了一个可以实现这个效果的插件： Markdown Preview Enhanced 。起初，我仅仅只是用到了它的公式渲染的功能，然而在查看了它的官方文档之后，我才发现它的功能远比我想象中的要强大。所以我决定写一篇博客介绍一下这一个插件，让大家也能更好地使用它。

### 1. 数学公式

有了数学公式的加成， Markdown 就变得更为方便好用了。 Markdown Preview Enhanced 中自带了 KaTeX 和 MathJax 公式的渲染，并且默认为 KaTeX ，所以我们不需要额外的配置就能够直接使用。

在插入公式时，我们可以使用 `$` 来表示行内公式，他会嵌入到一行文本中。例如下面这一段文本：

```
质能方程 $E=mc^2$，其中 $m=\frac{m_0}{\sqrt{1-v^2/c^2}}$ 为相对论质量， $m_0$ 为物体的静止质量。
```

将会被渲染成

质能方程 $E=mc^2$，其中 $m=\frac{m_0}{\sqrt{1-v^2/c^2}}$ 为相对论质量， $m_0$ 为物体的静止质量。

而使用 `$$` ，可以代表行间公式，它将会被单独渲染成一行，并且居中显示。例如下面这一段公式：

```
$$\sigma(z)=\frac{1}{1+e^{-x}}$$
```

将会被渲染成

$$\sigma(z)=\frac{1}{1+e^{-x}}$$

所以，我们只需要在一对 `$` 或者 `$$` 符号中写公式，就可以自动渲染，非常简单方便。如果大家对 LaTeX 的公式格式不大了解，可以参考一下 [常用数学符号的 LaTeX 表示方法](http://www.mohu.org/info/symbols/symbols.htm) 这一个页面。相信大家掌握后，将能够极大地提升自己记录公式的效率。

### 2. 引用文件

平时使用 Markdown 时，如果需要读入一张图片，我们需要使用 `![name](path)` 这样的形式，其实已经稍微显得有一些繁琐了。然而，假如我们要显示一段代码，在传统的 Markdown 语法中，我们只能将其粘贴在代码块中，这样不仅十分麻烦，也会让我们的 Markdown 文件显得比较臃肿。

但在 Markdown Preview Enhanced 这个插件下，这一切都不是问题！我们可以使用一个 `@import` 指令，完成几乎所有的操作。

我们可以用 `@import` 指令导入图片，甚至还可以设置大小：

```
@import "test.png"
@import "test.png" {width="300px" height="200px" title="my title" alt="my alt"}
```

当你导入一个代码文件时，它将会自动将其放于代码块中，并用对应的代码高亮渲染：

```
@import "test.cpp"
```

### 3. 将 Markdown 渲染成 Word/PDF/html

很多时候，我们写完 Markdown 文件之后，希望能够将它导出成 Word 或者是 PDF 的格式，方便其他人查看。

Pandoc 就可以实现将 Markdown 渲染成 Word 或者 PDF 的功能，并且 Markdown Preview Enhanced 插件中也集成了 Pandoc ，所以我们能够直接使用。

下面介绍一种非常常用的配置：在 Markdown 保存时，自动将其渲染为 PDF 文件。为了实现这个功能，我们只需要在 Markdown 文件的最开始插入下面的这一段 YAML 标记。

```yaml
---
title: How to use Markdown
author: Adelard
date: June 29, 2019
output: pdf_document
export_on_save:
    pandoc: true
---
```

其中的 `title` ， `author` 和 `date` 之类的都是可选的，如果不需要可以将其删除。如果要保存为 Word ，我们只需要将 `output` 后面的属性改为 `word_document` 即可。

这样，在保存 Markdown 文件时，它将会自动在相同的目录下渲染出一篇 PDF 格式的文档了，十分简便。

### 4. 在 Markdown 内运行代码

在 Python 的 IDLE 中，除了 PyCharm ， vscode 等等之外，还有一个独树一帜的存在，那就是 Jupyter Notebook。如果细说它的好处，其实主要是代码的实时运行，可以文档和代码一起呈现，还有就是能够直接显示出输出结果。

而在 Markdown Preview Enhanced 中，作者也添加了类似的功能。不过，在 Markdown 中运行代码的功能默认是关闭的，我们首先要在设置中开启 `enableScriptExecution`。

启用之后，我们试着添加一下下面的代码块。添加之后，在代码块的右上角就有全部运行和运行两个按钮了。运行代码后，我们会发现它能够直接将 matplotlib 绘制出来的结果显示在 Markdown 文档中。

    ```python {cmd=true matplotlib=true}
    import matplotlib.pyplot as plt
    import seaborn as sns
    sns.set_style("whitegrid")

    x = [1, 2, 3, 4]

    plt.plot(x)
    plt.show() # show figure
    ```

当然，它好像还是有一些 BUG ，譬如下方的代码在笔者的电脑上运行之后，并不能正确地显示:

    ```python {cmd=true matplotlib=true}
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns
    sns.set_style("whitegrid")

    x = np.linspace(-10, 10, 100)
    y = x ** 2 + x - 1
    plt.plot(x, y)
    plt.show() # show figure
    ```

### 结语

Markdown Preview Enhanced 的功能十分强大，其功能远不止上面介绍的那么一些。它甚至还支持在 Markdown 中直接绘制流程图，或是用 Markdown 绘制 PPT 。想要了解更多功能的话，大家可以在它的 [官方文档](https://shd101wyy.github.io/markdown-preview-enhanced/#/) 找到更详细的功能介绍。