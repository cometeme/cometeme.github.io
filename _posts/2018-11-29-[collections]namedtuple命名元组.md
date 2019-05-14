---
layout: post
title: '[collections] namedtuple 命名元组'
subtitle: '使用命名元组提升代码可读性'
date: 2018-11-29
categories: python
cover: '/assets/img/namedtuple-header.jpg'
tags: python python模块 collections
---

### 关于 collections 模块

python 自带的一些数据类型已经非常实用了，但在一些情况下，原生的数据结构并不能满足我们的需求。而在 python 中，想从底层定义一种数据类型又特别困难。因此， python 在它的 collections 模块中又为我们提供了几种新的数据类型，灵活的使用它们将会提升我们编写程序的效率。

### 1. 为什么要用命名元组

很多时候，我们需要用比较规范的形式将数据组织在一起，而对于简单的数据，使用结构体就有一些大材小用了。这个时候， `namedtuple` 这个类就可以很好地帮我们解决这一个问题。

### 2. 定义元组

命名元组的定义方式是： `namedtuple(typename, field_names)` ，其中 `typename` 是这一种命名元组的名字，而 `field_names` 是元素对应的名称，而且它可以为下面的任意一种形式：

-   ['x', 'y']

-   'x y'

-   'x, y'

介绍了申明的方法了之后，我们就拿一个最简单的例子来实战一下。在 python 程序中，我们经常用到“点”这样的类型，而这种时候命名元组的便捷性就体现出来了。

```python
from collections import namedtuple
p = namedtuple('Point', ['x', 'y'])
```

这样，我们就定义了一种新的命名元组，接下来我们就可以使用它了。

### 3. 使用元组

接下来，我们用定义好的命名元组创建两个新的元组：

```python
a = p(1, 2)
b = p(x=3, y=4)
print(a)
```

可以看到，初始化命名元组时，直接赋值与关键字赋值都是可以的，而这一段程序的输出结果是：

```python
Point(x=1, y=2)
```

前面的 `Point` ，就是我们刚刚标记的命名元组名称。这样就说明 `a` 和 `b` 这两个变量已经是 `Point` 类型的了。此时，我们试一下求一下这两个点之间的距离。

```python
dis = lambda a, b : ((a.x - b.x) ** 2 + (a.y - b.y) ** 2) ** 0.5
print(dis(a, b))
```

```python
2.8284271247461903
```

可以看到，这个时候我们就可以通过 `a.x` 这样的形式访问命名元组中的元素了。当然，我们还是可以像之前一样，使用 `a[0]` 来获取第一个元素。

### 4. 将一个列表导入到命名元组

有些时候，我们想将一个列表导入到命名元组中，这个时候我们就可以使用 `_make()` 方法来完成。

```python
l = [1, 2]
a = p._make(l)
print(a)
```

```python
Point(x=1, y=2)
```

有些时候，将列表转化为命名元组，会更有利于数据的处理，因此要灵活使用这一项功能。

### 5. 修改命名元组的值

标题看上去很令人疑惑--一个元组的值怎么能被修改呢。其实所谓的“修改”，就是新建一个修改后的元组，替换原有的元组，这样相当于间接地完成了元组的修改。而这一个功能可以通过内置方法 `_replace()` 来实现。

```python
a = p(x=1, y=2)
a._replace(x=3)
print(a)
```

```python
Point(x=3, y=2)
```

这样通过内置方法，我们就可以去“修改”命名元组的值了。

### 6. 命名元组的嵌套

比方说，我之前定义了一个命名元组叫 `Point`，而我现在想要定义一个正方形 `Square` ，它由 `x`, `y`, `a` 组成。那么这个时候，我们能否套用之前 `Point` 的定义，去组合出 `Square` 这个类型呢？

这当然是可以的，当我们使用命名元组的 `_fields` 这一个属性时，它将会返回这个元组组成的元素名称。

```python
>>> p._fields
('x', 'y')
```

而将这个返回值用于定义新命名元组时的参数设定，我们也就实现了命名元组的嵌套：

```python
sq = namedtuple('Square', p.fields + ('a',))
s = sq(1, 1, 4)
print(s)
```

```python
Square(x=1, y=1, a=4)
```

这样我们就实现了一个正方形的定义。那我们能不能用另方法，去定义实现一种新的命名元组"矩形" `Rectangle` ，它由两组 `x` ， `y` 坐标组成呢？

```python
rect = namedtuple('Rectangle', p._fields * 2)
```

```bash
Encountered duplicate field name: 'x'
```

可以看到，它提示我们有重复的属性 `x` ，因为命名元组要求参数唯一，这也是命名元组相较于结构体的一个缺陷。这时我们只能老老实实的用不同的变量名了。。。

```python
rect = namedtuple('Rectangle', 'x1 y1 x2 y2')
r = rect(0, 0, 4, 4)
print(r)
```

```python
Rectangle(x1=0, y1=0, x2=4, y2=4)
```

### 结语

正确地使用 namedtuple 可以使我们的代码变得更加直观，并且相较于结构体，它的定义更加的轻量化。不过当然，在某些情况下，我们还是需要使用结构体来完成更复杂的操作。因此我们在编程时应当作出合理的选择，这样才能发挥 python 最大的效率。
