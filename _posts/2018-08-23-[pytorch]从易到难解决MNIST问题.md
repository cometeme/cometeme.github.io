---
layout: post
title: '[pytorch] 从易到难解决 MNIST 问题'
subtitle: '机器学习入门问题'
date: 2018-08-23
categories: python
cover: '../../../assets/img/MNIST-header.jpg'
tags: pytorch MNIST
---

MNIST 可谓是机器学习的入门必讲的问题了。MNIST 数据集包含了手写的 0-9 的图片，其中有一个训练数据集和一个测试数据集。本文就采用“从易到难”的三种不同的模型，不断提升机器学习的正确率。

> 因为文章跨度较大，所以关于 pytorch 的基础内容在本文中不会介绍。基础内容可以参考 [pytorch入门](https://blog.csdn.net/u010223750/article/details/72915414)

### 1. Logistic Regression （逻辑回归）

Logistic Regression（逻辑回归）是用于解决分类问题的中非常常用的手段，而 MNIST 正好就是一个分类问题。 所以第一个要介绍的就是逻辑回归模型。

> 关于逻辑回归的原理和介绍，可以参考 [逻辑回归(logistic regression)的本质](https://blog.csdn.net/zjuPeco/article/details/77165974) 。

首先先要构造模型，因为输入的图片大小为 28*28 ，而最终分类完成后输出 10 种结果，所以我们先用 `nn.Linear(28 * 28, 10)` 创建一个全连接层。

模型创建成功后，我们还要编写训练与测试的函数，其中使用预置好的 `nn.CrossEntropyLoss()` （交叉熵损失）函数来计算损失，使用 `optim.SGD()` （随机梯度下降法）来进行优化。

```python
from __future__ import print_function
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.autograd import Variable
from torchvision import datasets, transforms


class logistic(nn.Module):
    def __init__(self):
        super(logistic, self).__init__()
        self.logstic = nn.Linear(28 * 28, 10)

    def forward(self, x):
        out = self.logstic(x)
        return out


def train(model, device, train_loader, optimizer, epoch):
    model.train()
    criterion = nn.CrossEntropyLoss()
    for batch_idx, (data, target) in enumerate(train_loader):
        data, target = data.to(device), target.to(device)
        data = Variable(data.view(-1, 28 * 28))
        target = Variable(target)
        optimizer.zero_grad()
        output = model(data)
        loss = criterion(output, target)
        loss.backward()
        optimizer.step()
        if batch_idx % log_interval == 0:
            print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(
                epoch, batch_idx * len(data), len(train_loader.dataset),
                100. * batch_idx / len(train_loader), loss.item()))


def test(model, device, test_loader):
    criterion = nn.CrossEntropyLoss()
    model.eval()
    test_loss = 0
    correct = 0
    with torch.no_grad():
        for data, target in test_loader:
            data, target = data.to(device), target.to(device)
            data = Variable(data.view(-1, 28 * 28))
            target = Variable(target)
            output = model(data)
            # get the index of the max log-probability
            pred = output.max(1, keepdim=True)[1]
            correct += pred.eq(target.view_as(pred)).sum().item()

    print('\nTest set:  Accuracy: {}/{} ({:.0f}%)\n'.format(
        correct,
        len(test_loader.dataset),
        100. * correct / len(test_loader.dataset))
    )


if __name__ == '__main__':
    batch_size = 64
    test_batch_size = 1000
    epochs = 10
    lr = 0.01
    momentum = 0.5
    seed = 1
    log_interval = 32

    use_cuda = torch.cuda.is_available()

    torch.manual_seed(seed)

    device = torch.device("cuda" if use_cuda else "cpu")

    kwargs = {'num_workers': 1, 'pin_memory': True} if use_cuda else {}
    train_loader = torch.utils.data.DataLoader(
        datasets.MNIST('./data', train=True, download=True,
                       transform=transforms.Compose([
                           transforms.ToTensor(),
                           transforms.Normalize((0.1307,), (0.3081,))
                       ])),
        batch_size=batch_size, shuffle=True, **kwargs)
    test_loader = torch.utils.data.DataLoader(
        datasets.MNIST('./data', train=False, transform=transforms.Compose([
            transforms.ToTensor(),
            transforms.Normalize((0.1307,), (0.3081,))
        ])),
        batch_size=test_batch_size, shuffle=True, **kwargs)

    model = logistic().to(device)
    optimizer = optim.SGD(model.parameters(), lr=lr,
                          momentum=momentum)

    for epoch in range(1, epochs + 1):
        train(model, device, train_loader, optimizer, epoch)
        test(model, device, test_loader)
```

进行了十轮测试后，最终的正确率大约达到了 92% 。

![logistic-result](../../../assets/screenshot/MNIST-1.png)

虽然作为最简单的示例，这个结果看起来还比较可观了。不过作为一个只需要识别数字的问题，这样的准确率还是远远不够的。

### 2. Multilayer Neural Network （多层神经网络）

在上一个模型，我们定义时用到了 `nn.Linear()` 这个函数，它的作用时构造一个全连接层。也就是说，逻辑回归的模型，其实就是一个简单的单层网络。

```python
class logistic(nn.Module):
    def __init__(self):
        super(logistic, self).__init__()
        self.logstic = nn.Linear(28 * 28, 10)
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        x = self.logstic(x)
        out = self.sigmoid(x)
        return out
```

可以看到，它将 28*28 种输入最终连接到 10 种输出，这样的确十分简单，但是拟合度就不够了。为了让正确率更高，我们需要多层网络来更好地进行分类。

> 神经网络的层数不是越多越好，层数变多容易造成“过拟合”，反而导致准确率下降。

这里我们就创建三层神经网络，让前一层的输出作为后一层的输入。

```python
class net(nn.Module):
    def __init__(self):
        super(net, self).__init__()
        self.layer1 = nn.Linear(28 * 28, 300)
        self.layer2 = nn.Linear(300, 100)
        self.layer3 = nn.Linear(100, 10)

    def forward(self, x):
        x = self.layer1(x)
        x = self.layer2(x)
        x = self.layer3(x)
        return x
```

这样一个网络其实能够运行了，但是我们还需要添加批标准化 `BatchNorm1d()` 与激活函数 `ReLU()` ，来增加网络的收敛速度和非线性，最终代码如下。要注意的是，最后一个输出层不需要添加函数。

```python
class net(nn.Module):
    def __init__(self):
        super(net, self).__init__()
        self.layer1 = nn.Sequential(
            nn.Linear(28 * 28, 300),
            nn.BatchNorm1d(300),
            nn.ReLU(True))
        self.layer2 = nn.Sequential(
            nn.Linear(300, 100),
            nn.BatchNorm1d(100),
            nn.ReLU(True))
        self.layer3 = nn.Sequential(
            nn.Linear(100, 10))

    def forward(self, x):
        x = self.layer1(x)
        x = self.layer2(x)
        x = self.layer3(x)
        return x
```

将这段代码替换掉原有的 logistic 类，就可以运行了。要注意在修改之后，要将程序中靠近末尾的 `model = logistic().to(device)` 改为 `model = net().to(device)` ，不然会报错。

![MNN-result](../../../assets/screenshot/MNIST-2.png)

可以看到准确度达到了 98% ，多层网络的提升还是很明显的。不过图像领域，多层神经网络并不是最好的选择。 CNN 模型的完善，给计算机视觉领域带来了突破性的发展。

### 3. CNN （卷积神经网络）

以上的神经网络，一张只有 28*28 的灰度图在第一层就需要 784 个神经元。如果图像更大，还包含 RGB 三通道，那么数据量就会更庞大。这个时候， CNN 就体现出它的优势了。关于 CNN 的原理与历史，本文就不再详细介绍。如果有兴趣可以参考这篇文章： [卷积神经网络(CNN)模型结构](https://www.cnblogs.com/pinard/p/6483207.html) 。

pytorch 中，只需要使用 `nn.Conv2d()` 就可以创建一个卷积层。我们的目标就是创建一个标准的 CNN 结构：

![CNN-structure](../../../assets/screenshot/MNIST-4.png)

在每次卷积后，我们都需要对其进行池化，防止数据量过多。因为这里没有使用 Variable ，我们使用 `F.nll_loss()` （可能损失负对数）函数来评估损失。关于这些函数的用法，大家可以自行搜索一下。因为代码与上面的有几个地方不同，所以这里直接贴出来：

```python
from __future__ import print_function
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms


class CNN(nn.Module):
    def __init__(self):
        super(CNN, self).__init__()
        self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = nn.Conv2d(10, 20, kernel_size=5)
        self.conv2_drop = nn.Dropout2d()
        self.layer1 = nn.Sequential(
            nn.Linear(320, 100),
            nn.BatchNorm1d(100),
            nn.ReLU(True))
        self.layer2 = nn.Sequential(
            nn.Linear(100, 10))

    def forward(self, x):
        x = F.relu(F.max_pool2d(self.conv1(x), 2))
        x = F.relu(F.max_pool2d(self.conv2_drop(self.conv2(x)), 2))
        x = x.view(-1, 320)
        x = self.layer1(x)
        x = F.dropout(x, training=self.training)
        x = self.layer2(x)
        return F.log_softmax(x, dim=1)


def train(model, device, train_loader, optimizer, epoch):
    model.train()
    for batch_idx, (data, target) in enumerate(train_loader):
        data, target = data.to(device), target.to(device)
        optimizer.zero_grad()
        output = model(data)
        loss = F.nll_loss(output, target)
        loss.backward()
        optimizer.step()
        if batch_idx % log_interval == 0:
            print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(
                epoch, batch_idx * len(data), len(train_loader.dataset),
                100. * batch_idx / len(train_loader), loss.item()))


def test(model, device, test_loader):
    model.eval()
    test_loss = 0
    correct = 0
    with torch.no_grad():
        for data, target in test_loader:
            data, target = data.to(device), target.to(device)
            output = model(data)
            # get the index of the max log-probability
            pred = output.max(1, keepdim=True)[1]
            correct += pred.eq(target.view_as(pred)).sum().item()

    print('\nTest set:  Accuracy: {}/{} ({:.0f}%)\n'.format(
        correct,
        len(test_loader.dataset),
        100. * correct / len(test_loader.dataset))
    )


if __name__ == '__main__':
    batch_size = 64
    test_batch_size = 1000
    epochs = 10
    lr = 0.01
    momentum = 0.5
    seed = 1
    log_interval = 32

    use_cuda = torch.cuda.is_available()

    torch.manual_seed(seed)

    device = torch.device("cuda" if use_cuda else "cpu")

    kwargs = {'num_workers': 1, 'pin_memory': True} if use_cuda else {}
    train_loader = torch.utils.data.DataLoader(
        datasets.MNIST('./data', train=True, download=True,
                       transform=transforms.Compose([
                           transforms.ToTensor(),
                           transforms.Normalize((0.1307,), (0.3081,))
                       ])),
        batch_size=batch_size, shuffle=True, **kwargs)
    test_loader = torch.utils.data.DataLoader(
        datasets.MNIST('./data', train=False, transform=transforms.Compose([
            transforms.ToTensor(),
            transforms.Normalize((0.1307,), (0.3081,))
        ])),
        batch_size=test_batch_size, shuffle=True, **kwargs)

    model = CNN().to(device)
    optimizer = optim.SGD(model.parameters(), lr=lr,
                          momentum=momentum)

    for epoch in range(1, epochs + 1):
        train(model, device, train_loader, optimizer, epoch)
        test(model, device, test_loader)
```

运行之后，可以看到，正确率达到了 99% ，不过用时相对也更多了。

![CNN-result](../../../assets/screenshot/MNIST-3.png)

其实这只是最普通的 CNN 模型，关于 CNN 模型还有许多拓展模型，比如 AlexNet ， GoogLeNet 等等。这些模型在处理 MNIST 问题时可以达到接近 100% 的正确率。如果大家感兴趣可以去了解一下。

### 结语与其他文档

这样，我们就通过 MNIST 这一个问题，介绍了三种不同的解答方法，其实这也是计算机视觉领域的进化史。在 CNN 模型之上，也有更加完善、高效的模型。希望大家能够在之后的学习中了解它们，并且做出更好玩的项目。
