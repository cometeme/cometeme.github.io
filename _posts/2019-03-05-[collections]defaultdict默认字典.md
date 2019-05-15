---
layout: post
title: '[collections] defaultdict 默认字典'
subtitle: '空元素含有默认值的字典类型'
date: 2019-03-05
categories: python
cover: '/assets/banner/defaultdict-header.jpg'
tags: python python模块 collections
---

之前介绍了 collections 模块中的 Counter 类，它是一种特殊的字典类型，在计数领域有种很多应用。而 collections 库中也有另一种特殊的字体类型—— `defaultdict` 。虽然它的功能相较于 `Counter` 较为简单，但是却也十分实用。

### 1. defaultdict 的优点

相比于传统的 `dict` 类型， `defaultdict` 在检索不存在的时，并不会出现 `KeyError` 错误，而会调用在定义时设定的默认函数。

除此之外， `defaultdict` 还有其他的功能吗？好吧，其实没有了。在其他方面，它都和一个标准的 `dict` 一样。虽然看上去这只是一个很小的改动，但实际上却能够为我们减少很多因为忘记判断元素是否存在的问题。

### 2. 创建一个 defaultdict 以及设置默认值

`defaultdict` 的定义非常简单，我们需要在括号内加上默认的 **函数** ：

> 要注意的是，和 `dict` 不同，括号内的内容将会被标记为默认值，所以形如 `dict({'a':1, 'b':2})` 这样的写法是不可行的

```python
from collections import defaultdict
dt = defaultdict(lambda: 0)
# 可以写成 dt = defaultdict(int)
dt['a'] = 11

print(dt['a'])
print(dt['c'])
```

这样我们就创建了一个默认值为 0 的 `defaultdict` ，在 `print(dt['c'])` 这个语句中，我们就会得到 0 这个默认结果。而不需要写成下方形式：

```python
if 'c' in dt:
    print(0)
```

> 对于 defaultdict ，在还没有访问某个不存在的元素之前，例如上面的例子，写 `'c' in dt` 会返回 False ，但在使用过 `dt['c']` 后 `'c' in dt` 会返回 True

在使用 `defaultdict` 时，除了 `lambda` 函数外，我们还能通过使用 `int` (返回 0 ) 、 `list` (返回空列表) 等。

### 3. 一个简单的应用

接下来做一个简单的应用：输入20个单词，将其按照首字母制作成字典，最后按照每个首字母输出。

```python
from collections import defaultdict
dt = defaultdict(list)

for i in range(20):
    word = input()
    dt[word[0]].append(word)

for c in range(97, 123):
    print('\n' + chr(c), end=':')
    for word in dt[chr(c)]:
        print(word, end='  ')
```

可以看到，这里就直接利用了 `defaultdict` 的默认值功能，所有未定义的元素都返回空列表，因此在执行 `dt[word[0]].append(word)` 这一句话时，并不需要提前创建空列表，而在输出时，所有没有内容的键都自动对应一个空的列表，所以可以直接使用 `for word in dt[chr(c)]` 同时处理存在的键和不存在的键。

### 结语

虽然 `defaultdict` 所带来的改变很小，但它却能在许多应用中减少 `dict` 的 `KeyError` 所带来的麻烦。不过当然设定默认值也有不好的一点，因为这样在写程序时容易忽略默认值带来的影响。所以在写程序时，我们需要灵活地选择 `dict` 与 `defaultdict` ，从而最高效地完成我们所需要的工作。
