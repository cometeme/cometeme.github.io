---
layout: post
title: 'PyInstaller 库'
subtitle: '听说你想把 python 程序打包成 exe ？'
date: 2018-08-05
categories: python第三方库
cover: 'assets/img/photo-1520500807606-4ac9ae633574.jpeg'
tags: python python库 python第三方库
---

python 编写的程序，除了运行速度的短板之外，还有一个问题就是它不能生成一个可执行文件的形式。当你编好一个 python 程序，想要发给朋友们看看时，他们还必须要装 python 的环境，之后才能运行。

作为一门解释性语言， python 无法在没有解释器的情况下运行。大牛们早就发现这样的不便捷，便造好了轮子！ python 其实有一个神奇的第三方库，就可以实现将 python 程序打包成 exe 的功能。它就是 `PyInstaller` ！

因为是一个第三方库，所以第一次使用时，需要先安装。如果提示权限不够，可能要用管理员（或前面加 `sudo` ）

`pip install PyInstaller`

等待安装成功后，将控制台切换到在你要编译的 python 程序的根目录下。例如

`cd /Users/cometeme/Documents/PythonCode`

接下来就可以使用 `pyinstaller -F example.py` 指令，将 `example.py` 打包成单个可执行文件了。

`INFO: Building EXE from out00-EXE.toc completed successfully.`

可以看到打包完成了。

其中 `pyinstaller` 这个指令还有其他可选参数，其中：

* -F 生成单个可执行文件
* -w 去掉控制台窗口，适用于GUI界面
* -i 设置图标

这样的话就可以把 python 程序打包成 exe 了，是不是特别简单呢。当然了， `PyInstaller` 不仅仅能做到产生 exe 文件，下面引用一段官方的介绍：

```
  PyInstaller is a program that freezes (packages) Python programs into stand-alone executables, under Windows, Linux, Mac OS X, FreeBSD, Solaris and AIX. Its main advantages over similar tools are that PyInstaller works with Python 2.7 and 3.3—3.6
```

打包完成之后，大家会发现目录下有许多文件。其中的一些其他文件就是给各种系统平台使用的。所以假如你在使用不同的平台， `PyInstaller` 都包含了对应的文件。而且 `PyInstaller` 库不仅仅能够将官方库的文件打包，他还支持许多主流的第三方库：

```
libraries like PyQt, Django or matplotlib are fully supported
```

怎么样，是不是很强大呢。如果你想深入了解它的原理，或是更广泛的应用，都可以在 [PyInstaller official website](http://www.pyinstaller.org) 上找到。
