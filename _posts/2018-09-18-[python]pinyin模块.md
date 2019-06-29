---
layout: post
title: '[python] pinyin 模块'
subtitle: '将汉字转化为拼音'
date: 2018-09-18
categories: python
cover: '/assets/image/pinyin-header.jpg'
tags: python python模块 python第三方模块
---

许多情况下，我们需要将一段汉字转换为拼音。比如我们可以用拼音来进行排版，或者是将拼音输出到其他程序，生成语音。

一般将汉字转化成拼音这样的功能，我们需要一个字典来实现。比如 Mandarin.dat 。不过在 PyPI 中，已经有不少模块将其封装，而这里要介绍的就是 pinyin 模块。

### 1. pinyin 模块的安装

我们可以使用 pip 或 conda 指令安装 pinyin 模块。

```bash
$ pip install pinyin
```

或

```bash
$ conda install pinyin
```

### 2. 进行简单的拼音转换

在 pinyin 模块中，我们只需要一个简单的 `get()` 函数，就可以返回拼音的符号

```python
>>> import pinyin
>>> pinyin.get('你好')
'nǐhǎo'
```

如果使用 `delimiter` 参数的话，可以设置两个拼音之间的分隔符。比如可以设置为空格：

```python
>>> pinyin.get('你好', delimiter=" ")
'nǐ hǎo'
```

很多时候， `ǐǎ` 这样的符号并不适合计算机来读取。 `format` 参数可以设置输出拼音的方式，比如我们设置为 `strip` 参数去掉注音，或者使用 `numerical` 将注音以数字的方式放在最后：

```python
>>> pinyin.get('你好', format='strip', delimiter=" ")
'ni hao'
```

```python
>>> pinyin.get('你好', format='numerical', delimiter=" ")
'ni3 hao3'
```

### 3. 获得每个拼音的首字母

使用 `get_initial` 就可以输出每个文字拼音的首字母。不过它会自动加空格。

```python
>>> pinyin.get_initial('你好')
'n h'
```

### 4. 甚至还有中译英功能

从项目 Github 的介绍来看， pinyin 这个模块甚至还有中文翻译功能。不过作者也说这个功能是测试用的，而且也很久没有更新的。所以只把官方的介绍放一下：

```python
>>> import pinyin.cedict
>>> pinyin.cedict.translate_word('你')
['you (informal, as opposed to courteous 您[nin2])']
>>> pinyin.cedict.translate_word('你好')
['Hello!', 'Hi!', 'How are you?']
>>> list(pinyin.cedict.all_phrase_translations('你好'))
[['你', ['you (informal, as opposed to courteous 您[nin2])']], ['你好', ['Hello!', 'Hi!', 'How are you?']], ['好', ['to be fond of', 'to have a tendency to', 'to be prone to']]]
```

这个功能只是一个试验功能，所以只是放着玩一玩的，如果大家要更精确的汉译英，可以查找 python 如何使用在线翻译。

### 5. 结语

pinyin 模块就这样介绍完了。虽然比较简单，但也十分使用。很多时候，文本转拼音还是一件比较简单的事，但是要将拼音转化为文本就可以使用另外一个模块的功能： [Pinyin2Hanzi - Github](https://github.com/letiantian/Pinyin2Hanzi)
