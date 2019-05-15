---
layout: post
title: '[python] 为 pip 设置国内源'
subtitle: '让 pip 下载更流畅'
date: 2018-08-15
categories: python
cover: '/assets/image/pip-header.jpg'
tags: python pip
---

国内使用 pip 下载 python 软件包总是一件令人头疼的事。下载速度慢不说，还有可能会失败。这个问题导致我必须要用很多时间来重新安装包。

其实之前在使用 Debian 的 `apt-get` 指令时，也常常出现类似的情况。不过 `apt-get` 可以通过更换镜像源来加速。抱着一试的态度，我发现 pip 果然也有国内镜像源。在 pip 时，只需要在后面加上 `-i <source>` 指令就可以实现。

### 1. 常用的源

因为之前更换 `apt-get` 的源时用的就是清华源和阿里云的源，所以我优先寻找到了这两个。

阿里云 <https://mirrors.aliyun.com/pypi/simple/>

清华 <https://pypi.tuna.tsinghua.edu.cn/simple>

### 2. 在使用 pip 指令时切换源

接下来我们试一试切换源，下载一个 tensorflow 模块

```bash
$ sudo -H pip install tensorflow -i https://mirrors.aliyun.com/pypi/simple/
```

> 如果你使用的源为 http ，那么会提示源不被信任。你只需要按照提示，在后面加上 --trusted-host <host> 就可以了

可以看到，下载速度比直接下载快了许多，像 tensorflow 这种比较大的模块都可以很快下载。

不过如果每次使用 pip 都需要输入一次，也有些繁琐。

### 3. 在文件中配置默认源

不同的系统平台的配置方法不同，第一次设置时需要新建目录和文件。

Windows `C:\Users\<username>\pip\pip.ini`

MacOS `/Library/Application Support/pip/pip.conf`

Linux `/.config/pip/pip.conf`

在新建的配置文件中写入以下内容：

```python
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
```

这样就可以在之后默认使用设定的源了。
