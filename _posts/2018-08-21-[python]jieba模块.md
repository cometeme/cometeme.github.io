---
layout: post
title: '[python] jieba 模块'
subtitle: '给中文的文本分词'
date: 2018-08-21
categories: python
cover: '/assets/image/jieba-header.jpg'
tags: python python模块 python第三方模块
---

在文本处理时，英文文本的分词一直比中文文本要好处理许多。因为英文文本只需要通过空格就可以分割，而中文的词语往往就很难从句子中分离出来。这种时候我们往往需要一个“词典”来实现分词，而寻找“词典”又是件非常麻烦的事。

不过， python 强大的第三方模块中早有了解决方案。在 PyPI 上面搜索“中文分词”，第一个出现的就是 jieba 模块。其实 jieba 模块的官方文档已经足够详细了，所以这里就对其进行一定的精简，只介绍几个常用的函数。

### 1. 使用 pip 安装 jieba 模块

在第一次使用时，我们需要先使用 pip 指令安装 jieba 这个第三方模块：

```bash
pip install jieba
```

### 2. lcut() -- 最常用的分割模式

`lcut()` 这个函数只有简单的两个参数： `lcut(s, cut_all=False)` ，而它在切分后会返回一个字符串。其中 `s` 是传入的中文字符串，而 `cut_all` 这个参数默认为 `False` ，默认为“精确模式”，而如果设置为 `True` ，就是“全模式”。那么这两个模式有什么区别呢？我们可以查看下官方文档中的示例：

```python
import jieba

seg_list = jieba.lcut("我来到北京清华大学", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # 全模式

seg_list = jieba.lcut("我来到北京清华大学", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # 精确模式
```

```plain
Full Mode: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学
Default Mode: 我/ 来到/ 北京/ 清华大学
```

可以看到，精确模式下不会有重复的词，而全模式虽然会有重复（歧义），但也在一定程度上可以避免精确分词时的失误。

但是，上面的切分方式虽然在日常做文本统计时足够使用，但是它却不适合搜索引擎使用。因为它的切分还包含了一些较长的词语，并没有对其进行继续的切分。这个时候我们就需要使用“搜索引擎模式”进行切分。

### 3. lcut_for_search() -- 搜索引擎模式

`lcut_for_search()` 这个函数一般在使用时只需要填写第一个参数，就是待分词的字符串。继续看官方文档的示例：

```python
seg_list = jieba.lcut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
print(", ".join(seg_list))
```

分词的结果：

```plain
小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造
```

这样分解之后的列表就比较适用于搜索引擎使用了。但是在统计时并不适合这样使用，因为会有重复，还会有歧义。

### 4. 为 jieba 设置多进程分词

> 并行分词并不适用于 Windows 平台，如果想要尝试的话可以使用 Linux 虚拟机

当文本较短时，分词所使用的时间还比较可观。但是当你从一个文件中读取一大串的文本时，处理的时间就会十分长。这种情况下 jieba 可以通过设置多进程的方式来并行分词。这样可以提高分词效率。

其中，我们可以通过 `enable_parallel()` 这个函数来开启多进程分词，而使用 `disable_parallel()` 可以关闭多进程。

```python
jieba.enable_parallel(4) # 开启并行分词模式，参数为并行进程数
jieba.disable_parallel() # 关闭并行分词模式
```

> 在文本过短时，多进程反而会降低运行效率。所以一般只有在拆分大量文本时再使用并行分词

### 结语与其他文档

以上就介绍了 jieba 模块的简单使用方法了，这样基本就能完成常用的分词操作。不过 jieba 模块也支持自定义词典，如果你发现分词效果不够好，那么也可以查阅文档解决： [Github - jieba](https://github.com/fxsjy/jieba)
