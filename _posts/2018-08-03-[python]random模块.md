---
layout: post
title: '[python] random 模块'
subtitle: '生成随机化的数据，抑或是打乱一个列表'
date: 2018-08-03
categories: python
cover: '/assets/image/random-header.jpg'
tags: python python模块
---

random 模块是 python 自带模块中非常常用的一个模块，它可以产生许多随机化的操作。无论你是做网页开发，还是写普通的算法程序，随机数的生成都起到了很重要的作用。

### 1. 随机数种子 seed() 与 random() 函数

按照其他语言的惯例，在导入 random 模块之后，我们首先需要设定一个随机数种子 `seed(num)` 。其中 `num` 就是要设置的种子。为什么要设置随机数种子呢？其实计算机的随机数是一种“**伪随机数**”，它并不能做到真正的随机，而是用一种算法通过随机数种子计算出一个“随机的”序列。所以当随机数种子相同的时候，出现的随机数的序列是一样的。在设定了随机数种子后，我们就可以用 `random()` 函数来输出随机数。下面我们先演示随机数种子相同的情况：

```python
import random
random.seed(3)
print(random.random())
```

运行两次，输出结果是相同的

`0.23796462709189137`

`0.23796462709189137`

前面说其他语言使用 `random()` 前要定义随机数种子，但是 python 与其他语言不同。当你省去 `seed()` 中的参数时，它会自动使用系统当前时间作为种子，因为时间基本不会重复，所以就可以保证随机的唯一性（不会像指定种子那样会重复）。 python 中使用 random 模块时，还可以不加 `seed()` 这一语句，它也会自动将当前时间设置为随机数种子。所以我们测试下面的代码：

```python
import random
print(random.random())
```

可以看到，这样的话两次输出就不同了，表面上看起来就具备了随机性。

`0.826457253856883`

`0.656398726587975`

### 2. randint() 、 randrange() 、 uniform() 、 getgetrandbits()

细心的朋友可能会发现， `random()` 输出的值都在0-1之间，事实上它的取值范围是 `[0, 1)` 的浮点数，那么假如想要输出 `[1,9]` 的整数，要怎么办呢？

也许有的人会想到：

```python
int(random.random() * 9) + 1
```

如果你想到了上面的代码，恭喜你，基础功不错。不过在 python 中，这就不够“简洁优雅”了。正确的使用方法是

```python
random.randint(1, 9)
```

`randint(a, b)` 的作用是产生一个 `[a, b]` 的整数。与这个不同的是， `randrange(m, n[, k = 1])` 这个函数可以产生一个 `[m, n)` 之间以 `k` 为步长的整数，其中参数 `k` 是可以省略的，默认值为1。使用上面几个函数时，要**注意开闭区间**。

`randint(a, b)` 和 `randrange(m, n[, k = 1])` 这两个函数产生的都是整数。如果要随机产生一段区间的浮点数的话，就可以用 `uniform(a, b)` 这一个函数，它可以生成一个区间为 `[a, b]` 的随机小数。除此之外，还有一个 `getrandbits(k)` 的函数，它可以产生k比特长的随机整数，在使用固定位数16进制数或者2进制数时，这个函数将更加好用。

上面讲了这么多都是和生成随机数有关，不过 random 模块不仅仅能产生随机数，它还能实现更多的功能……

### 3. choice() 与 choices() - 随机选择

比如说你想做一个随机点名的程序，你将所有学生的姓名放在一个列表中。假如 random 模块只能实现生成随机数的功能的话，你还必须要将产生的随机数一一分配（比如说分配学号），这样并不直观。这个时候， python 就给我们提供了一个很好的选择 -- `choice(seq)` 。这个函数可以实现从一个列表中随机选择一个内容并返回。例如以下的代码：

```python
import random
ls = ['David', 'Mike', 'Jack']
print(random.choice(ls))
```

`David`

如果要选出多个元素的话，只需要在 choice 后面加个 s ，`choices(seq[, k = 1])` ，其中seq仍为待输入的序列，在后加 `k = n` 则可以挑选出 `n` 个元素生成一个新的列表。不过这个元素是会重复的。下面就遇到了这个问题：

```python
import random
ls = ['David', 'Mike', 'Jack']
print(random.choices(ls,k = 2))
```

```python
['Jack', 'Jack']
```

### 4. shuffle() - 随机打乱一个序列

接下来就是一个更有趣的函数了， `shuffle(seq)` ，它可以将一个序列打乱。注意它会将数列**原地打乱**，意思就是说，运行完毕后 `seq` 保存的就是打乱后的序列，而将这个函数的结果直接输出或者赋值给其他变量都是**不可行**的。

**错误** 用法：

```python
import random
ls = ['David', 'Mike', 'Jack']
print(random.shuffle(ls))
```

返回值是 `None` ，所以正确用法为

```python
import random
ls = ['David', 'Mike', 'Jack']
random.shuffle(ls)
print(ls)
```

返回值是 `['Mike', 'Jack', 'David']`

### 结语

以上就是本教程的全部内容了。 random 模块还有其他拓展的内容，包括以上介绍的 `choices` 、 `shuffle` 函数都还有额外的参数可供选择。具体可以参考 [9.6. random — Generate pseudo-random numbers](https://docs.python.org/3/library/random.html) 。
