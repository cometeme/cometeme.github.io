---
layout: post
title: '[collections] Counter 计数器'
subtitle: '使用更简单的方法来给元素计数'
date: 2018-12-07
categories: python
cover: '/assets/banner/Counter-header.jpg'
tags: python python模块 collections
---

在做文本统计分析的时候，我们经常会需要进行“计数”这样一个环节。而如果直接使用字典，代码难免会变得复杂。其实， collections 模块中早就给我们定义好了 Counter 类，它可以很方便地实现计数的功能。

### 1. 创建一个 Counter 对象

只需要使用 `Counter()` 就可以创建一个 Counter 对象了，其中的参数可以有许多种写法：

-   列表  `Counter(['apple', 'boy', 'apple'])`


-   字典  `Counter({'apple':2, 'boy':1})`


-   字符串（每一个字符视为一个元素）  `Counter('aabca')`


-   关键字形式  `Counter(apple=2, boy=1)`

接下来以一个小例子对比一下使用普通的字典与 Counter 统计词频的区别。在统计之前，我们先对文本进行一些处理：

```python
s = 'A boy and a girl'
s = s.lower().split()
```

切分完文本之后，我们先展示使用普通的字典进行词频统计的方法：

```python
d = {}

for word in s:
    if word in d:
        d[word] += 1;
    else:
        d[word] = 1;

print(d)
```

```python
{'a': 2, 'boy': 1, 'and': 1, 'girl': 1}
```

虽然 python 的语法已经比其他编程语言简单不少，但是这样的结构看起来还是有些繁琐。而如果使用 Counter 对象的话，我们只需要用一行语句就可以完成：

```python
c = Counter(s)
print(c)
```

```python
Counter({'a': 2, 'boy': 1, 'and': 1, 'girl': 1})
```

可见， Counter 的确能够简化我们对计数的操作。不过除了能够在一开始的统计上简化代码外，它还有许多方便的方法可以使用。

### 2. 获取对应键的值

Counter 获取值的方法与字典类似，只需要用 `[key]` 就可以获取对应的值。不过它与原先的字典不一样的是，如果 `key` 不存在，它不会有 `KeyError` 的报错，而是会返回 `0` 。

```python
a = Counter('aabc')
print(a['d'])
```

```python
0
```

在原先使用字典时，在查找键之前，我们都需要先进行检验。而使用 Counter 类型就十分方便。

### 3. Counter 的加减

其实， Counter 里面已经实现了对加减号的重载。它可以实现将两个计数器里面的对应值相加或相减。

```python
a = Counter('aabc')
b = Counter('bbcd')
c = a + b
d = a - b
print(c)
print(d)
```

这段程序运行的结果如下：

```python
Counter({'b': 3, 'a': 2, 'c': 2, 'd': 1})
Counter({'a': 2})
```

可以看到，当一个元素的计数被减至0时，它便不会再出现，而不需要单独处理“个数为负”的情况。

不过，如果你想要负的结果，那么你可以选择 `subtract()` 方法。

```python
a = Counter('aabc')
b = Counter('aaaa')
a.subtract(b)
print(a)
print(+a)
print(-a)
```

`+a` 与 `-a` 可以返回 Counter 中正的部分与负的部分（不过 `-a` 返回的负值的相反数）。这一个操作十分常用。

```python
Counter({'b': 1, 'c': 1, 'a': -2})
Counter({'b': 1, 'c': 1})
Counter({'a': 2})
```

### 4. most_common() - 取出最多的 n 个元素

词频统计中，我们往往需要取出最大的几个元素，用于后续的分析。而这项功能， Counter 也帮我们做好了。我们只需要调用 `most_common(n)` 方法，就可以取出 Counter 对象中最多的 `n` 个元素，并返回一个元组类型。

```python
a = Counter('abracadabra')
print(a.most_common(3))
```

元组内的每一个元素都分别是对应的键值，并且已经按照降序排列：

```python
[('a', 5), ('b', 2), ('r', 2)]
```

仅仅一行，我们就可以获取 Counter 计数器中最多的几项了，这极大的简化了我们的代码量。

### 总结

上面的介绍展示了 Counter 计数器的常用方法。其实在了解它之前，我一直都是用字典做词频统计，因此也更发现 Counter 计数器在这一类问题上的简便性。大家也可以尝试一下自己用字典实现这一些功能，这样便能够更好地理解 Counter 的工作原理。
