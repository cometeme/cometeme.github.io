---
layout: post
title: '[Atom] snippets - 自定义代码补全'
subtitle: '强大好用的内置代码补全插件'
date: 2019-04-22
categories: Atom
cover: '/assets/img/snippets.jpg'
tags: Atom 插件
---

### 前言

在平时写代码时，自动补全一直是一个非常实用的功能。然而，很多代码块并**没有内置的自动补全**，这时就需要我们自己设置。或是在另一些时候，我们会用到**模版**程序，例如 ACM 竞赛时需要用到的宏定义，如果能够通过输入一行指令，自动补全整个模版，那将会方便许多。

近些年来，代码编辑器都自带了自定义代码补全的功能。在 Visual Studio Code 和 Atom 等编辑器中，我们主要可以通过内置的 snippets 插件来自定义自动补全的方式。那么接下来，我将会介绍在 Atom 中的 snippets 配置方法。

### 1. 文件位置

不同于 vsc ，在 Atom 中修改 snippets 需要手动打开配置文件。

在 Windows 平台下，文件的位置在：

`C:\Users\{User Name}\.atom\snippets.cson`

而在 macOS 操作系统下，配置文件在：

`~/.atom/snippets.cson`

使用编辑器打开文件，我们就可以看到里面有不少注释：

```text
# Your snippets
#
# Atom snippets allow you to enter a simple prefix in the editor and hit tab to
# expand the prefix into a larger code block with templated values.
#
# You can create a new snippet in this file by typing "snip" and then hitting
# tab.
#
# An example CoffeeScript snippet to expand log to console.log:
#
# '.source.coffee':
#   'Console log':
#     'prefix': 'log'
#     'body': 'console.log $1'
#
# Each scope (e.g. '.source.coffee' above) can only be declared once.
#
# This file uses CoffeeScript Object Notation (CSON).
# If you are unfamiliar with CSON, you can read more about it in the
# Atom Flight Manual:
# http://flight-manual.atom.io/using-atom/sections/basic-customization/#_cson
```

粗略的阅读完注释之后，我们就可以开始我们的配置环节了。

### 2. snippet 文件的配置

在注释下面，我们输入 `snip` 并按下回车，编辑器就自动为我们生成了一个配置：

```text
'.source.js':
  'Snippet Name':
    'prefix': 'Snippet Trigger'
    'body': 'Hello World!'
```

其中，每一部分的作用如下：

```text
'.source.文件后缀（类型）':
  '代码补全的名称（没有实际作用）':
    'prefix': '触发的指令'
    'body': '替换成的代码'
```

例如，现在我们要实现一个简单的 C++ 自动补全模版，我们就可以这样填写：

```text
'.source.cpp':
    'ACM Template':
        'prefix': 'my_template'
        'body': '''
                #include <iostream>
                #include <algorithm>
                #include <cmath>

                using namespace std;

                #define INF 0x3f3f3f3f
                #define EXP 1e-8

                #define ll long long

                int main()
                {
                    ios::sync_with_stdio(false);


                    return 0;
                }
                '''
```

因为要在 C++ 文件中使用，所以我们将最开始的内容改为了 `.source.cpp` 。而为了保证 body 中的代码能够实现换行，并且不会出现 \\ 的转义错误，我们最好**使用三个单/双引号**将其围起来。

保存之后，我们打开一个 C++ 文件，输入 `my_template` 并按回车 / `Tab` 时，便会自动填充整个模版。

### 3. snippets 的高级使用技巧

填充模版并不能够完美的应用于所有的场景。很多时候，像输入 `for` 这样的语句时，我们希望编辑器能够自动补全整个代码块，并且将其中一些部分变为可替换。当我们按下 Tab 时，便会自动切换到下一个区块内，便于我们填写。那么这样的功能怎么实现呢？

首先，先从最简单的一种自动补全开始： C++ 中 `include` 的 `<>` 补全。

因为在前面我们已经添加过一个 `.cpp` 的模版了，所以添加第二个模版时，我们**不需要重复写 `.source.cpp` 这一行**，不然会导致报错。

```text
'include with <>':
    'prefix': 'include'
    'body': 'include <$1>'
```

尖括号中间的 `$1` 是 snippets 一种标记，它在补全时不会出现，但是当补全完成后，光标将会自动移动到这个位置。而 `$2` ， `$3` 便代表着补全之后，按下 `Tab` 便会将光标移动到这些标记的位置。

保存并打开一个 C++ 文件，当我们输入 `#include` 时，按下回车或是 `Tab` ，会发现编辑器自动帮我们补全了尖括号，并且将光标设置在了两个尖括号之间，这样就便于我们输入了。

可是，这样的自动补全没有任何提示信息。对于 `include` 这样较为简单的也许还可以，但是对于复杂的代码补全就不行了。我们可以先用 `for` 举一个例子。我们可以先在配置文件中插入如下的代码：

```text
'Auto Complete for':
    'prefix': 'for'
    'body': '''
    for($1; $2; $3)
    {
        $4
    }
    '''
```

当我们在 C++ 文件中输入 `for` 时，将会自动补全整个 `for` 的表达式。输入完一部分后，按 `Tab` 便会跳转到下一部分，最终会跳转到花括号之间。

然而实际使用的情况下，这样的代码补全可读性十分一般。如果能在代码补全的时候给予一定的提示，那将会变得更人性化。好在 snippets 也有这样的功能。在之前，我们使用 `$1` 标记光标的位置。但是如果我们使用 `${1:text}` 这样的形式，便能够在代码补全时提供提示。

所以我们可以将刚才的 `for` 自动补全改为以下形式：

```text
'Auto Complete for':
    'prefix': 'for'
    'body': '''
    for(${1:Start}; ${2:Condition}; ${3:Step})
    {
        ${4:Code}
    }
    '''
```

这样，在使用 `for` 的自动补全时，会默认生成 `Start` 等内容。但是补全后或是当我们按下 `Tab` 切换位置时，编译器会将这些提示文本选中，当输入新的内容时就会将这些提示文本替换，这样一来既不会影响正常的编程体验，也可以起到一定的提示功能了。

### 结语

在许多时候，自动补全能够完善我们的编程体验，将一些繁琐的、重复的工作简化。因此，希望大家能够在学习或是工作时更好地去使用 snippets 这一个插件，灵活设置跳转等功能，让 snippets 成为一个编程时的得力助手。
