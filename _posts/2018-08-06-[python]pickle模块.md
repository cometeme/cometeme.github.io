---
layout: post
title: '[python] pickle 模块'
subtitle: '将数据腌制成完美的泡菜'
date: 2018-08-06
categories: python
cover: '/assets/image/pickle-header.jpg'
tags: python python模块
---

今天要给大家介绍的，是 pickle 模块。通过词典我们可以知道， pickle 这个词的意思是“泡菜”。听了这么个名字，大家是不是更加疑惑了？难道我们要把程序放进一个缸里，让他自己发酵？哈哈，当然不是这样。

pickle 模块的产生是为了解决文件读取中的一大问题：如何方便地保存和读取数据。许多时候我们写入一个文件，只是为了将数据保存，为了方便下一个程序读取它。而一般存储的文件都是以文本形式，这样就容易导致互相的不兼容。所以产生了两种常用的标准存储形式： **CSV** 和 **JSON** 。下面先简单介绍一下它们：

### 1. CSV 的原理

CSV 可以较为方便地存储一个一维或者二维的列表。它用**逗号**分隔不同的数据，用**换行**来表示下一组数据。以下是 [Wiki-CSV](https://en.wikipedia.org/wiki/Comma-separated_values) 上的介绍：

> In computing, a comma-separated values (CSV) file is a delimited text file that uses a comma to separate values. A CSV file stores tabular data (numbers and text) in plain text. Each line of the file is a data record. Each record consists of one or more fields, separated by commas. The use of the comma as a field separator is the source of the name for this file format.

CSV 使用起来较为简单，python 中也有[官方的 CSV 模块](https://docs.python.org/3/library/csv.html)，我们用代码也可以实现。比如有下面这样一个 CSV 格式的字符串。

```python
'12,24,46'
```

在 python 中，我们只需要用到字符串中的 `split` 方法，便可以实现将数据恢复为列表：

```python
ls = '12,24,46'.split(',')
```

但是要将列表转化为 CSV 格式就稍微复杂些了。我们常用 `‘,’.join(ls)` 这样的方法将列表转化为字符串，但是 `join` 方法只支持所有元素**全部为字符串**的列表，如果列表中有数字等类型就会报错。而且二维列表就需要循环来输出每一行。这个实现方法不难，但是代码有些冗长，所以不贴上来了。如果各位有兴趣可以尝试自己写一写。

但是 CSV 的存储还有一些问题，比如它**不能**存储类似于对象/结构体这样的复杂的类型。而且如果数据中**含有逗号**这个字符，那么 CSV 便**无法**正确地将数据分隔。遇到这样的问题， JSON 就是另一种解决方案。

### 2. JSON 与 pickle 的区别

JSON 是一种较为通用的数据存储结构，它可以在多种语言之间相互使用。 JSON 的语法较为复杂，这里就不再详细介绍。JSON 能够实现**任意类型**的数据存储，这使得它比 CSV 有更广的适用性。 python 也带有 [JSON 的官方模块](https://docs.python.org/3/library/json.html)，也可以比较方便地实现读取存储功能，但是对于不多的数据， JSON 便有些“大材小用”了（JSON 相较于 pickle 还有一点优点就是它可以方便地可视化，因为 pickle 保存后的文件为二进制格式，无法直接用文本编辑器直接打开；但这样相对来说 pickle 的文件较为安全，它被直接打开或是被中途修改的可能性就更小）

然而假如你仅仅只需要在 python 中存储几个数据。使用上面两种方法便也有些过于复杂。如果你不需要用到通用的数据存储， pickle 就是一个最好的选择。

### 3. pickle 的常用函数

pickle 模块只需要用简单的 `dump(obj, file)` 函数就可以实现写入文件，而使用 `load(file)` 函数便可读取数据。（其中 `obj` 是待写入的对象，`file` 是文件对象。 `dump()` 与 `load()` 函数都可以**多次**使用，会在原来的基础上继续读写，这样可以存储多个数据。）

比如说我们下面创建两个实例 `example1.py` 、 `example2.py` 并且尝试在它们之间传输一个列表和一个数据。为了增大难度，我们将列表变为三维，混合了数据类型，并且将其中一部分改为元组。

```python
#example1.py
import pickle
f = open("abc.pickle", 'wb+')
ls = [[(23, 29), (22, 79)], 'hello', '35']
num = 3.1415926
pickle.dump(ls, f)
pickle.dump(num, f)
```

```python
#example2.py
import pickle
f = open("abc.pickle", 'rb')
newls = pickle.load(f)
newnum = pickle.load(f)
print(newls)
print(newnum)
```

我们先运行 `example1.py` ，这个时候便会产生一个叫做 `abc.pickle` 的文件。文件名和后缀怎么取名都可以。我们先尝试用文本编辑器打开这个文件，便会显示以下的内容。

```python
]q(]q(KKqKKOqeXhelloqX35qe.G@	!MJ.
```

之前说过， pickle 输出的数据是**二进制（流）**的格式，它不能够被直接以文本形式打开。所以在刚刚两个实例中，都要使用 `wb` 、 `rb` 写入、读取文件。

现在我们打开 `example2.py` 并运行，可以看到控制台成功输出了 `example1.py` 程序中写入的结果，甚至连列表和元组的类型都没有改变：

```python
[[(23, 29), (22, 79)], 'hello', '35']
3.1415926
```

pickle 模块的神奇之处在于它可以让你免去文件“I/O”中的转换环节。让你能够更快地完成数据的存储功能。当然， `pickle.load()` 函数的预设是 `ASCII` 编码，所以假如读取后中文出现了乱码（应该会直接报错），这时我们修改 `encoding` 参数就可以了。

```python
pickle.load(f, encoding = 'gb2312')
```

### 4. dumps() 与 loads() - 不一样的功能

在自动补全时，我发现 pickle 还有 `dumps()` 和 `loads()` 这两个函数。一开始我以为它做的是将多个数据存入一个文件中（这个用多个 `dump()` 函数就可以实现），然而还是发现 Too Young 了。查阅了官方文档之后，发现这两个的作用与 `dump()` `load()` 不同。 `dump()` 是直接将生成好的二进制（流）保存到文件中，而 `dumps()` 是直接返回一个二进制（流）。（没有写入文件，而是可以赋给其他变量）

平时，我们可能并不会去使用这两个函数。不过其实在之后的使用中，它们可以用于在数据模块中存储 python 数据类型。例如 Redis 的键值并不能支持全部的 python 类型，但是如果用 `dumps()` 将一个二进制流传入，读取的时候再用`loads()` 完整取出，就可以简单地实现多种数据格式的存储。如果感兴趣的话可以看一下我的另外一个文档：[[python] redis 模块](/python/2018/08/python-redis模块.html)

### 结语

学习了 pickle 的功能，我们可以猜想它的取名可能影射的是作者想要实现将数据“完整地放入”并“完整地取出”的功能，不过这也有可能是作者的一种情怀。看完 pickle 这个模块之后，你是不是对 python 存储数据有了更深的认识呢？当然如果大家想要更加深入地了解这一个模块，在 [12.1. pickle — Python object serialization](https://docs.python.org/3.7/library/pickle.html) 中也有安全提示、相关模块、支持类型等更详细的文档。
