---
layout: post
title: '[python] redis 库'
subtitle: '简单地实现 key-value 存储'
date: 2018-08-17
categories: python
cover: '../../../assets/img/redis-header.jpg'
tags: python python库 python第三方库
---

Redis 可以说是 NoSQL （非关系型数据库）中较为流行的一种数据库，虽然相较于 MongoDB ， Redis 的检索算法不够全面，但是它的性能更好，对数据持久化的处理也较优。

所以在建立数据库时，也经常会用到 Redis ，而且一般用它和其他数据库相互组合使用。相较于 SQL ， Redis 可以很好地处理缓存类的部分，并且更易实现分布式，从而提升服务器的访问效率。

### 1. 安装 redis 和 python 第三方库

Redis 的服务器文件我们需要到它的官网上去下载：[Redis.io](https://redis.io) 。

下载完成后，我们解压文件，然后打开控制台，切换到解压后的目录下，然后输入 `make` 指令，经过一段时间的安装后， Redis 就安装成功了。

安装好了服务器之后，我们需要安装 python 的 redis 第三方库，来实现对 Redis 连接的支持。

```bash
$ pip install redis
```

### 2. 连接 Redis 数据库

首先我们先以普通模式运行 Redis 数据库，打开终端，可以看到一个 Redis 的图标。

```bash
$ redis-server
```

![Redis_bash](../../../assets/screenshot/redis-1.png)

> 在调试完成后，我们应该使用 conf 配置文件打开 Redis 服务器，不然会有安全性隐患。

在 python 中，我们可以通过定义一个 Redis 对象来实现对 Redis 数据库的连接。这里我们先使用 IDLE 的交互式编程模式来完成。

```python
>>> import redis
>>> database = redis.Redis()
```

只需要一个简单的函数，我们就能连接我们的服务器了。

其中， `Redis` 这个函数其实还有可选参数:

-   host
    设置 Redis 服务器的 IP 位置，默认为 `localhost`

-   port
    设置 Redis 服务器的端口，默认为 `6379`

-   db
    设置使用的数据库标号，默认为 `db=0`

-   password
    设置 Redis 服务器的密码，如果没有设置密码就不用填

### 3. 对数据库进行操作

Redis 的操作非常简单，最基础的只有两个函数， `set()` 和 `get()` 。

-   redis.set(key, value)
    将 key 对应的值设置为 value ，如果设置成功就返回 `True`

-   redis.get(key)
    返回 key 对应的值，如果 key 不存在，则返回 None

让我们先来试一试这两个函数：

```python
>>> database.set('username', 'cometeme')
True
>>> database.get('username')
b'cometeme'
```

可以看到，虽然的确成功返回了我们存入的信息，但是前面多了一个 `b'` ，这代表的是这是 byte 形式的值。那么怎么样才能还原成正常的数据呢？

其实只需要加个 `decode` 函数就可以恢复为正常的数据。

```python
>>> database.get('username').decode()
'cometeme'
```

假如你存了许多键值对，想要查看所有设置过的键值对，那么你就可以使用 `redis.keys()` 这个函数来返回所有键。

```python
>>> database.keys()
[b'passwd', b'username']
```

这样返回的又都是带 `b'` 的数据了，如果想要将它全部复原的话，我们可以使用 `for` 循环来完成解码，这样就解决问题。，但是每一次使用这些功能时，都需要设置这么多的步骤。其实我们可以自己做一个模块，从而能够在其他程序中更简单地完成这些操作。

### 4. 写一个简单的 Redis 操作模块

为了方便之后的使用，我们可以将一些 Redis 数据库的操作写成一个模块，这样可以便于之后调用。我们可以新建一个叫 redisOperation.py 的文件，并且定义一个类。

```python
import redis


class redisOperation():
    def __init__(self, host='localhost', port=6379, db=0,password=""):
        self.database = redis.Redis(host=host, port=port, db=db, password=password)
        print("Successfully connect to Redis Server.")

    def setData(self, key, value):
        self.database.set(key, value)

    def getData(self, key):
        data = self.database.get(key)
        if data is None:
            return None
        else:
            return data.decode()

    def getKeys(self):
        byteKeys = self.database.keys()
        rawKeys = []
        for key in byteKeys:
            rawKeys.append(key.decode())
        return rawKeys
```

首先在初始化的位置，我们设定了默认参数，这样可以以最简单的方式来测试一个服务器。 `setData()` 函数没有任何改变，但是 `getData()` 和 `getKeys()` 函数都加入了解码的过程，这样只需要调用这个函数就可以直接返回原始值，减少了重复的工作。

接下来我们就可以使用自己写的库了：

```python
from redisOperation import redisOperation


r = redisOperation()
r.setData('username', 'cometeme')
print(r.getData('username'))
```

但是，在有一类情况下，单纯使用 `decode()` 会出现问题：当你存入的数据不是字符串类型，但是使用 `decode()` 之后返回的是一个字符串。如果你想要使用它，你就必须知道当初存进去的是什么类型，再决定是否需要使用 `eval()` 函数，这无疑增加了非文本类型的存储（特别是文本与非文本类型混合存储）时的解码难度。

```python
>>> r.setData('username', [10,20,90,(30,10,29)])
>>> r.getData('username')
'[10, 20, 90, (30, 10, 29)]'
```

其实问题在于，我们存入一个值，而取出时为 byte 类型。这意味着我们存入时会先将信息变为 byte 类型，就是这一步导致之后我们无法区分数据类型。那怎样才能让我们的数据“**原样进，原样出**”呢？等等，是不是想到了我们之前介绍的 pickle 库？[[python] pickle 库](../../../python/2018/08/python-pickle库.html)

### 5. redis + pickle

其实要使用 pickle 库完成数据的存储，只需要简单的更改上面的代码。在介绍 pickle 库时，我提到了有两个不常用的函数： `dumps()` 和 `loads()` ，这两个就是用来生成与还原二进制流的。虽然我说他们不常用，但是在这里他们就派上了用场。我们只需要在存入时通过 `dumps()` 将任意的 python 变量转化为二进制流，而在取出时用 `loads()` 将其完整的取出，那就可以实现任意类型的 python 变量存储。

更新后的代码如下，其实只改动了很小一部分。

```python
import redis
import pickle


class redisOperation():
    def __init__(self, host='localhost', port=6379, db=0, password=""):
        self.database = redis.Redis(
            host=host, port=port, db=db, password=password)
        print("Successfully connect to Redis Server.")

    def setData(self, key, value):
        self.database.set(key, pickle.dumps(value))

    def getData(self, key):
        data = self.database.get(key)
        if data is None:
            return None
        else:
            return pickle.loads(data)

    def getKeys(self):
        byteKeys = self.database.keys()
        rawKeys = []
        for key in byteKeys:
            rawKeys.append(key.decode())
        return rawKeys
```

这样就可以完美地实现在 Redis 数据库中进行各种数据类型的存储了。

```python
>>> r.setData('username', [10,20,90,(30,10,29)])
>>> r.getData('username')
[10, 20, 90, (30, 10, 29)]
```

> 其实如果平时设置 key 时使用字符串，那么在存入时我们的 key 就不需要用 pickle 库来编码，这样也可以增加效率。不过如果你想要将 key 值设置成更多的变量类型，那么就可以在存储 key 时增加 dumps() 函数

### 结语与其他文档

如此我们就介绍完了 python 与 redis 的连接与使用，相信在看完了篇文章后，你能够更完全地了解 Redis 这一个数据库，并且能够在之后的项目里更好地使用它。
