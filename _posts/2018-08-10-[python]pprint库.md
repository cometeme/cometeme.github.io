---
layout: post
title: '[python] pprint 库'
subtitle: '整齐地输出列表或是字典'
date: 2018-08-10
categories: python
cover: '../../../assets/img/pprint-header.jpg'
tags: python python库
---

有时候我们需要在控制台中输出一个列表或者是字典，然而预置的 `print()` 函数又不能整齐地进行输出。作为一个强迫症，我经常会在需要时自定义一个函数来进行输出。不过直到我前几天看到 python 的官方文档，我发觉我又在浪费时间了。。。

其实， pprint 库就能很好地解决上面的问题。 pprint 是 "PrettyPrinter" 的缩写。看到名字大家应该就知道它是干什么的吧。它有许多强大的可选参数。不过它的使用方式与之前介绍的库不同。在使用 pprint 的输出功能之前，它要求我们先：

### 1. 定义一个 PrettyPrinter 对象

为什么要定义一个 PrettyPrinter 对象呢？其实是因为 PrettyPrinter 的参数过多，如果你直接使用输出语句的话，每一次输出可能要在后面加上许多的参数。而在一个程序中，这些参数不大会改变。所以只需要一开始声明一个对象，之后我们就可以省去重新配置参数的过程，而是直接输出了。

那么首先，我们要用 `PrettyPrinter` 函数来定义一个对象。

```python
import pprint
pp = pprint.PrettyPrinter(width = 80)
```

这样我们就定义好了一种输出的对象了。大家可能很好奇其中的 `width` 参数是做什么用的。其实 `PrettyPrinter` 中的参数可不止这一个。

-   `width`
    定义最大的宽度（如果超过这个宽度会自动换行）（默认的宽度为80）

-   `indent`
    定义缩减的空格数（默认为1）

-   `compact`
    如果为 `True` 则将同一种元素压缩到一行（默认为 `False` ）

-   `depth`
    定义最大的嵌套数。如果列表中超过一定的深度就以 `...` 显示

看完上面的介绍，是不是有一点蒙？没事，下面我们就要介绍 `pprint()` 函数，用它就可以进行输出。这样我们就可以测试一下各个参数的功能了。

### 2. pprint 函数 与 PrettyPrinter 的参数效果

在上面定义好了 PrettyPrinter 对象之后，我们就可以用它来进行输出了。我们调整几个参数，看看它的实际效果。

```python
import pprint
ls = ['abc', 'def', 'ghj', ['abc', 'def']]
pp = pprint.PrettyPrinter(width = 20)
pp.pprint(ls)
```

先不加 `compact = True` ，理论上第一组 `abc` 和 `def` 其实是可以输出在同一行的。但是因为 `pprint` 希望让显示的层次变得清晰，它并不会这样输出。而是变成下面的那样：

```python
['abc',
'def',
'ghj',
['abc', 'def']]
```

可以看到，前三组层次并列的数据被输出在了不同的行，而最后两组被输出在了同一行。

如果我们加上 `compact = True` 这个参数呢？

```python
import pprint
ls = ['abc', 'def', 'ghj', ['abc', 'def']]
pp = pprint.PrettyPrinter(width=20, compact=True)
pp.pprint(ls)
```

可以看到，在满足宽度限制的情况下，前两个数据被输出在了同一行，而第三个数据因为宽度超出而自动分配到了下一行。

```python
['abc', 'def',
'ghj',
['abc', 'def']]
```

其实看到了上面这样的显示结果，一般来说不推荐大家使用 `compact = True` 这个参数，因为它会让可读性变得有些差。（不过如果你的列表是一维的，或者数据都十分整齐，那么 `compact = True` 的确可以让列表的输出变得更紧凑）

那么看了上面几个参数，还有 `indent` 参数没有介绍到。我们还是以不包含 `compact` 为例，用同样的数据进行测试：

```python
import pprint
ls = ['abc', 'def', 'ghj', ['abc', 'def']]
pp = pprint.PrettyPrinter(indent = 2, width=20)
pp.pprint(ls)
```

输出在每组数据之前加上了两个空格，其实 `indent` 这个参数可以让数据看起来不那么密集，不过加不加就看个人喜好了。

```python
[ 'abc',
  'def',
  'ghj',
  ['abc', 'def']]
```

最后是 `depth` 参数了。其实它也是特别的实用。（只不过除了在做网络类的程序外，其他地方不是经常用到）

比如我用一个四层嵌套的列表，然后我将最大深度 `depth` 设置为 `2` ，来测试一下输出效果：

```python
import pprint
ls = ['abc', 'def', 'ghj', ['abc', ['abc', ['abc', 'def'], 'def'], 'def']]
pp= pprint.PrettyPrinter(indent = 2, depth = 2, width=20)
pp.pprint(ls)
```

```python
[ 'abc',
  'def',
  'ghj',
  [ 'abc',
    [...],
    'def']]
```

可以看到过于深层嵌套的部分就被省略掉了。这样其实也有助于我们阅读结果。

这大概就是 `pprint` 函数的功能了。

### 结语与其他文档

pprint 库最重要的两个函数以及他们的参数都已经介绍完了。其实它还有一些其他的函数。就比如如果你不想直接输出列表，而是要获得一个字符串，那么 `pformat` 函数就可以帮助你。它还有 `isreadable` 和 `isrecursive` 函数来判断可读性和递归性。如果大家水平好的话可以自己研究一下这个库其他函数的作用，在这里我就贴上链接： [pprint — Data pretty printer](https://docs.python.org/3/library/pprint.html)
