---
layout: post
title: '[python] venv 库'
subtitle: '创建一个轻量化的虚拟环境'
date: 2018-08-07
categories: python
cover: '../../../assets/img/venv-header.jpg'
tags: python python库
---

在使用 python 制作网页的过程中，我们往往需要先将网页的文件“**虚拟化**”。虚拟化其实就是将当前文件下程序的运行环境与整个系统的环境隔离。那么为什么我们要将一个项目虚拟化呢？

### 1.不进行虚拟化会产生的问题

在平时使用 python 时，有可能会遇到这几个常见的问题：

1.当运行的项目处于**不同版本**时（如 python 2.7/3.7 ），要通过切换 python 解释器的版本来运行程序（或要使用 `python2/3` `pip/pip3` 等指令来对应不同的版本）。

2.有时做一个项目要用到许多第三方库，但是其他项目基本**不会用**。如果直接 `pip install` 到系统中，项目删除后清除安装过的库会很麻烦。

3.做完一个项目后，你希望能够**不再一次**安装它依赖的库，就能在另外一个系统上直接运行。

4.你的项目暂时运行在一个旧版的第三方库上，而新版的第三方库不兼容你所写的程序。你既希望能够让原来的项目**在旧版本上**继续正常运行，又希望能够在另一个项目中使用新版本。

如果你遇到过上面这四个问题，并且希望能够改善这一些繁琐的配置过程，那么你就可以尝试对你的项目进行“**虚拟化**”。

### 2.创建一个虚拟化项目

python 自带了一个非常简便的虚拟化库 - venv 。在 python 3.5 及之前的版本，创建一个虚拟化项目的指令为：

```python
  python -m venv <directory>
```

而在 3.6 之后的版本中，指令变成了

```python
  python3 -m venv <directory>
```

其中在 3.4 及之前的版本中，初始化一个环境是**不会自带 pip** 的。而在 3.4 之后，可以添加 `--without-pip` 这个参数来去掉 pip 包。

当然，venv还有 `--copies` 参数，它就可以在创建时把系统已有的库复制进去。不过一般这个不常用。（因为系统里装的库可能已经很多了）如果真的要使用系统中的库，请参考第四小节的设置。

### 3.进入/退出虚拟化环境

成功创建了虚拟化的环境之后，我们需要先**进入**这个环境（不然 pip 还是会装在项目外面）。我们可以使用以下指令来进入环境。其中 `<venv>` 是你项目文件夹的路径。


- MacOS 或 Linux

  - bash/zsh	`$ source <venv>/bin/activate`

  - fish	`$ . <venv>/bin/activate.fish`

  - csh/tcsh	`$ source <venv>/bin/activate.csh`

- Windows

  - cmd.exe	`C:\> <venv>\Scripts\activate.bat`

  - PowerShell	`PS C:\> <venv>\Scripts\Activate.ps1`

```python
  cometeme$ source /Users/cometeme/Documents/example/bin/activate
  (example)cometeme$
```

可以看到前面多了一个(example)的提示，说明我们已经进入到虚拟环境中了。在虚拟环境里，我们可以像正常一样操作里面的文件，或是安装第三方库，只不过这一些都只会影响到本地的内容了。

当我们想要退出虚拟环境时，我们可以输入 `deactivate` 这个指令来退出。

### 4.设置虚拟环境

在虚拟环境的根目录下，我们可以看到一个叫 `pyvenv.cfg` 的文件。用记事本打开后可以看到这几行内容：

```python
  home = /Library/Frameworks/Python.framework/Versions/3.7/bin
  include-system-site-packages = false
  version = 3.7.0
```

其中， `home` 指的是系统中 python 库的安装位置。除非你自定义过了，不然这个默认值就是对的。

`include-system-site-packages` 这个参数可以设置是否开启“**引用系统中的库**”的功能。如果开启了，那么当你就可以直接使用系统中已经装过的第三方库。（但是你在虚拟环境下装的库就不能被其他地方的程序使用）

`version` 可以指定 python 的版本，前提是你必须要安装这个版本。（比如你只装了 python3.6 ，那么你设置成 2.7 就会出错）

### 结语与其他文档

短短的几十行， venv 的这个库的一些简单使用方法就已经介绍完了，简洁却又方便，这一直是 python 的特性。其实除了可以在控制台中使用外，它还可以作为一个模块在程序中使用。不过一般情况下我们只需要用控制台就可以了。如果大家还想深入研究，可以查看 [29.3. venv — Creation of virtual environments](https://docs.python.org/3/library/venv.html) 。
