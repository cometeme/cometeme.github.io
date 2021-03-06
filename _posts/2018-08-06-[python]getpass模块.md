---
layout: post
title: '[python] getpass 模块'
subtitle: '如何才能在控制台中让用户输入的密码隐藏？'
date: 2018-08-06
categories: python
cover: '/assets/image/getpass-header.jpg'
tags: python python模块
---

有些情况下，我们需要在控制台中让用户输入密码，而这个时候我们希望输入的内容被**隐藏**。回想一下 python 中的标准输入输出函数，好像并没有一个函数能够实现这样的功能。但是别慌， python 早就为我们准备了一个标准模块 - getpass 模块。这个模块最常用的其实只有一个函数，那就是：

### getpass() - 隐式输入

`getpass(str = "Password:")` 这个函数非常方便地实现用户的**隐式输入**。它的用法和 `input(str)` 这个函数相似。其中 `str` 指的是提示用户输入的字符串。在被省略时默认输出"Password:"

下面看一个实例：

```python
import getpass
username = input("Username:")
password = getpass.getpass()
print("Hello:", username, ". Your password is ", password)
```

```python
Username:cometeme
Password:
Hello: cometeme . Your password is123123
```

可以看到，在 `Password:` 这一行内，我们输入的所有数据都不会被显示出来。所以说这样特别适合用于输入一些**密码**类的隐私数据。而 `input()` 后输入的数据在输入时是会被**显示**出来的，这样方便用户校对，所以它们各有利弊，也有自己的适用范围。

### 结语

可能你们会很好奇，为什么 getpass 模块中只有一个函数。其实它里面不止一个函数 -- 它有两个，哈哈。还有一个函数就是 `getuser()` ，它会检测所有可能存储用户名的环境变量，如果不为空就将其返回。只不过这个函数在平时编程时并不是很常用罢了。如果想要更详细地了解这个函数，可以查阅 [16.9. getpass — Portable password input](https://docs.python.org/3/library/getpass.html) 。
