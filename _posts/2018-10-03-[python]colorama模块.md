---
layout: post
title: '[python] colorama 模块'
subtitle: '改变控制台输出文本的颜色'
date: 2018-10-03
categories: python
cover: '/assets/image/colorama-header.jpg'
tags: python python模块 python第三方模块
---

除了使用 PyQt 这样的图形化开发框架外，基本上 python 程序都是跑在控制台中的。很多时候，单纯使用黑白的文字不能很好地突出我们要显示的信息。有时候我们需要将错误的提示使用红色标注，而将成功的提示设置为绿色。这时候，基础的控制台显示操作就不能很好地满足我们了。虽然我们可以使用 ANSI 来标注输出字体的颜色，但是这样需要记忆它的格式，不是特别方便。

这个时候，我们就可以通过 colorama 这个第三方模块，用简单的语法实现字体颜色的控制。接下来就来看看它有多神奇。

### 1. 安装 colorama 模块

```bash
$ pip install colorama
```

如果你使用 Anaconda 这样的环境，它就会预装 colorama 模块。但是如果使用的是 miniconda ，这个时候就需要安装一下：

```bash
$ conda install colorama
```

### 2. 使用 colorama 模块

在使用 colorama 的字体颜色模式之前，需要先使用 `init()` 函数进行初始化。以下就是一个简单的实例：

```python
from colorama import init, Fore, Back, Style
init()
print(Fore.RED + 'some red text')
print(Back.GREEN + 'and with a green background')
print(Style.BRIGHT + 'and in bright text')
print(Style.RESET_ALL)
print('back to normal now')
```

其中， `init()` 函数可以传入一个参数： `autoreset` 。默认值为 False ，如果设置为 True ，它就会在每一次输出语句之后自动清空格式。

```python
from colorama import init, Fore, Back, Style
init(autoreset=True)
print(Fore.BLUE + 'some blue text')
print(Back.CYAN + 'cyan background')
print(Style.DIM + 'in dim text')
print('auto set to normal now')
```

而 `Fore, Back, Style` 这三个类型，分别可以设置显示字体的显示风格。它需要连接到待输出字符串的前面。其中 Fore 是前景色（字体颜色）， Back 是背景色， Style 可以改变字体的显示模式，同时也可以清空字体风格。这三个属性可以设置的参数如下：

-   Fore: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.

-   Back: BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE, RESET.

-   Style: DIM, NORMAL, BRIGHT, RESET_ALL

### 结语

只需要通过几个参数，就可以用 colorama 模块进行简单的颜色控制了。其实如同 `Fore.RED` 这样的模式只是保存了一个 ANSI 的编码。官方文档中就有比较详细的解释： [colorama-PyPI](https://pypi.org/project/colorama/)。如果想要更深层次地自定义，其实也可以自己将 ANSI 进行一定的封装来使用更多的颜色。
